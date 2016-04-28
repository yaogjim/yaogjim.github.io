---
layout: post
---
在ssh中添加jersey shiro quartz支持
最近一个App需要用到推送任务，因为推送的内容不是自己生成，所以需要定时检测内容更新，有更新就读取内容，然后推送到App。

推送服务端用的avos的push（为什么用avos，无非想对avos的rest api混个脸熟。），提供iOS版本的接口，sever端只支持rest。

用jersey client连接 avos的rest（用httpclient连接也可以，而且简单多了，不需要和jackson扯。为什么用jersey，理由同上，和jersey 混脸熟。），在整合到现有的ssh项目项目时，最痛苦的时jeysey依赖的包，因为需要用到json解析，又选择jackson，so，又要添加jackson包，一堆包在么有maven管理的项目中简直就是噩梦。闲话不是，贴一段jersey包的maben配置

		<dependency>
			<groupId>org.glassfish.jersey.containers</groupId>
			<artifactId>jersey-container-servlet</artifactId>
			<version>2.12</version>
		</dependency>
		<dependency>
			<groupId>org.glassfish.jersey.media</groupId>
			<artifactId>jersey-media-json-jackson</artifactId>
			<version>2.12</version>
		</dependency>


有了rest操作功能，就要把它们打成定时任务，这就要用到quartz。以前自己曾写过任务管理功能，taskmanager，参考quartz，逻辑实现简单多了。
使用quartz，就三步：
1、在spring配置文件中加配置


    <bean id="schedulerFactoryBean" class="org.springframework.scheduling.quartz.SchedulerFactoryBean">  
        <property name="triggers">  
            <list>  
                <ref local="newPostTriggerQuartz"/>  
                <ref local="serverDownTriggerQuartz"/>  
            </list>  
        </property>  
    </bean>   
    <!-- 配置触发器 -->  
    <bean id="newPostTriggerQuartz" class="org.springframework.scheduling.quartz.CronTriggerBean">  
        <property name="jobDetail" ref="notifyNewPost"/>  
        <!-- 每隔1分钟执行一次 -->  
        <property name="cronExpression" value="0 0/1 * * * ?"/>  
    </bean>    
    <bean id="serverDownTriggerQuartz" class="org.springframework.scheduling.quartz.CronTriggerBean">  
        <property name="jobDetail" ref="notifyServerDown"/>  
        <!-- 每隔1分钟执行一次 -->  
        <property name="cronExpression" value="0 0/5 * * * ?"/>  
    </bean>               
           
    <!-- 配置触发器调度的bean -->  
    <bean id="notifyNewPost" class="org.springframework.scheduling.quartz.MethodInvokingJobDetailFactoryBean">  
        <!-- 引用一个bean -->  
        <property name="targetObject" ref="checkNewPost"/>  
        <!-- 指定目标bean的方法 -->  
        <property name="targetMethod" value="checkNewPosts"/>  
    </bean>     
    <bean id="notifyServerDown" class="org.springframework.scheduling.quartz.MethodInvokingJobDetailFactoryBean">  
        <!-- 引用一个bean -->  
        <property name="targetObject" ref="checkServerStatus"/>  
        <!-- 指定目标bean的方法 -->  
        <property name="targetMethod" value="checkServerStatus"/>  
    </bean>  
      
    <!-- 被调度的bean -->  
    <bean id="checkNewPost" class="com.salehao.task.CheckServerStatus">  
    </bean>  
    <bean id="checkServerStatus" class="com.salehao.task.CheckServerStatus">  
    </bean>
    
    
 2、实现要调度的任务－－之前实现的rest操作
 
 3、在项目中添加quartz依赖包
 
 
 		<dependency>
			<groupId>com.fasterxml.jackson.core</groupId>
			<artifactId>jackson-core</artifactId>
			<version>2.2.3</version>
		</dependency>
		<dependency>
			<groupId>com.fasterxml.jackson.core</groupId>
			<artifactId>jackson-databind</artifactId>
			<version>2.2.3</version>
		</dependency>

		<dependency>
			<groupId>org.quartz-scheduler</groupId>
			<artifactId>quartz</artifactId>
			<version>1.8.6</version>
		</dependency>
 
 
 本来事情差不多完成了，后来想反正用jersey了，就把rest 服务也加上去吧。
 于是，写了个简单的rest服务：
 


 	@Path("/book") 	
	public class BookService {
	
	@Autowired(required = true)
	private DemoService demoService;
	@GET
	@Path("/query/{title}")
	// @Consumes(MediaType.APPLICATION_FORM_URLENCODED)
	@Produces(MediaType.APPLICATION_JSON)
	// http://127.0.0.1:8080/jerseyDemo/rest/speak/querybook/kktitle
	public Book getBookByPath(@PathParam("title") String title) {
		Book book = new Book();
		book.setName("this is a kk book");
		book.setTitle(title);
		return book;
	}
	。。。
	
然后整合到spring上去的时候，@Autowired 就不灵了。上面的demoService一直时null。
要让jersey整合到spring中，得加jersey－spring，注意版本和exclusion，一不小心，包和包之间冲突，各种异常，还是无法查来源得异常。

		<dependency>
			<groupId>org.glassfish.jersey.ext</groupId>
			<artifactId>jersey-spring3</artifactId>
			<version>2.12</version>
			<exclusions>  
				<exclusion>  
                    <groupId>com.sun.jersey</groupId>  
                    <artifactId>jersey-servlet</artifactId>  
                </exclusion> 
                <exclusion>  
                    <groupId>org.springframework</groupId>  
                    <artifactId>spring</artifactId>  
                </exclusion>  
                <exclusion>  
                    <groupId>org.springframework</groupId>  
                    <artifactId>spring-aop</artifactId>  
                </exclusion>
                <exclusion>  
                    <groupId>org.springframework</groupId>  
                    <artifactId>spring-asm</artifactId>  
                </exclusion>
                <exclusion>  
                    <groupId>org.springframework</groupId>  
                    <artifactId>spring-core</artifactId>  
                </exclusion>  
                <exclusion>  
                    <groupId>org.springframework</groupId>  
                    <artifactId>spring-web</artifactId>  
                </exclusion>  
                <exclusion>  
                    <groupId>org.springframework</groupId>  
                    <artifactId>spring-beans</artifactId>  
                </exclusion>  
                <exclusion>  
                    <groupId>org.springframework</groupId>  
                    <artifactId>spring-context</artifactId>  
                </exclusion>  
            </exclusions>  
		</dependency>
		

然后就想着把简单的身份验证加上在rest上吧，于是shiro来了。
shiro很简单了，照着guide搭，基本没问题。
唯一头痛是和ssh整合，ssh已经有url pattern了，再加上shiro filter的pattern，然后它们冲突了：访问rest/speak/hell的时候，会跑到struts的action解析上，告知没有对应的action。废话，本来就是rest服务，哪来的action。
因为没有在原来的ssh中加＊.action的后缀，在url pattern中所以filter都是/*，所以在struts的filter中加了一个"/rest/"的检测，然后action和rest就解开了。

最后要提一点关于maven管理包依赖的。
加了maven 支持后的项目，包简单、清晰非常多了。
但同时，在增加一些plugin等依赖时，可能会增加很多已经存在包的其他版本，带来新的冲突。
解决的办法是在eclipse中打开pom，选择dependency hierarcky  右侧根据字母排序查看，看看有没有同一artifactId多个版本的包的，有的话就exclusion一下。


