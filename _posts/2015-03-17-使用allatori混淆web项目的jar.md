---
layout: post
---
使用allatori混淆web项目的jar的配置文件
 
####  1、将allatori包放在lib下，执行
 java -jar ./lib/allatori.jar config.xml 
 
####  2、配置文件
 
 
 	<config>
    <jars>
        <jar in="shopper.jar" out="../obf-shopper.jar"/>
    </jars>

    <classpath>
    <jar name="activemq-all-5.10.0.jar"/>
    <jar name="activemq-camel-5.10.0.jar"/>
    <jar name="activemq-console-5.10.0.jar"/>
    <jar name="activemq-jaas-5.10.0.jar"/>
    <jar name="activemq-pool-5.10.0.jar"/>
    <jar name="activemq-protobuf-1.1.jar"/>
    <jar name="activemq-spring-5.10.0.jar"/>
    <jar name="activemq-web-5.10.0.jar"/>
    <jar name="activiti-bpmn-converter-5.14.jar"/>
    <jar name="activiti-bpmn-layout-5.14.jar"/>
    <jar name="activiti-bpmn-model-5.14.jar"/>
    <jar name="activiti-camel-5.14.jar"/>
    <jar name="activiti-cdi.jar"/>
    <jar name="activiti-common-rest-5.14.jar"/>
    <jar name="activiti-cxf-5.14.jar"/>
    <jar name="activiti-diagram-rest-5.14.jar"/>
    <jar name="activiti-engine-5.14.jar"/>
    <jar name="activiti-explorer-5.14.jar"/>
    <jar name="activiti-json-converter-5.14.jar"/>
    <jar name="activiti-ldap-5.14.jar"/>
    <jar name="activiti-modeler-5.14.jar"/>
    <jar name="activiti-mule-5.14.jar"/>
    <jar name="activiti-osgi-5.14.jar"/>
    <jar name="activiti-rest-5.14.jar"/>
    <jar name="activiti-simple-workflow-5.14.jar"/>
    <jar name="activiti-spring-5.14.jar"/>
    <jar name="antlr-2.7.6.jar"/>
    <jar name="aopalliance-1.0.jar"/>
    <jar name="asm-3.3.1.jar"/>
    <jar name="asm-commons-3.3.jar"/>
    <jar name="aspectj-1.7.1.jar"/>
    <jar name="aspectjrt-1.7.0.jar"/>
    <jar name="aspectjweaver-1.7.0.jar"/>
    <jar name="backport-util-concurrent.jar"/>
    <jar name="c3p0-0.9.1.2.jar"/>
    <jar name="cglib-2.2.2.jar"/>
    <jar name="cglib-nodep-2.2.jar"/>
    <jar name="com.d_project.qrcode.jar"/>
    <jar name="com.google.api.translate.jar"/>
    <jar name="commons-codec-1.4.jar"/>
    <jar name="commons-collections-3.1.jar"/>
    <jar name="commons-dbcp.jar"/>
    <jar name="commons-email-1.1.jar"/>
    <jar name="commons-fileupload-1.2.2.jar"/>
    <jar name="commons-httpclient-3.0.1.jar"/>
    <jar name="commons-io-2.0.1.jar"/>
    <jar name="commons-lang-2.4.jar"/>
    <jar name="commons-lang3-3.1.jar"/>
    <jar name="commons-logging-1.1.1.jar"/>
    <jar name="commons-pool.jar"/>
    <jar name="configdebug-1.0.jar"/>
    <jar name="dom4j-1.6.1.jar"/>
    <jar name="dwr-1.1.1.jar"/>
    <jar name="ehcache-1.5.0.jar"/>
    <jar name="fetion-java-api.jar"/>
    <jar name="freemarker-2.3.19.jar"/>
    <jar name="gson-2.2.4.jar"/>
    <jar name="hibernate-jpa-2.0-api-1.0.0.Final.jar"/>
    <jar name="hibernate-validator-4.0.0.GA.jar"/>
    <jar name="hibernate3.jar"/>
    <jar name="itext-xtra-5.2.1.jar"/>
    <jar name="itextpdf-5.2.1.jar"/>
    <jar name="jackson-all-1.7.4.jar"/>
    <jar name="javamail.jar"/>
    <jar name="javassist-3.11.0.GA.jar"/>
    <jar name="jaxen-1.1-beta-6.jar"/>
    <jar name="jdom.jar"/>
    <jar name="jedis-2.1.0.jar"/>
    <jar name="joda-time-1.6.2.jar"/>
    <jar name="json-lib-2.1.jar"/>
    <jar name="json-lib-2.4-jdk15.jar"/>
    <jar name="json_simple-1.1.jar"/>
    <jar name="jsr107cache-1.1.jar"/>
    <jar name="jta-1.1.jar"/>
    <jar name="log4j-1.2.15.jar"/>
    <jar name="mail-1.4.1.jar"/>
    <jar name="mongo-2.10.1.jar"/>
    <jar name="mongo-java-driver-2.12.1.jar"/>
    <jar name="mybatis-3.1.1.jar"/>
    <jar name="mybatis-generator-core-1.3.2.jar"/>
    <jar name="mybatis-spring-1.1.1.jar"/>
    <jar name="mysql-connector-java-5.1.5-bin.jar"/>
    <jar name="ognl-3.0.5.jar"/>
    <jar name="qrcode.jar"/>
    <jar name="Qrcode_swetake.jar"/>
    <jar name="quartz-2.2.1.jar"/>
    <jar name="quartz-jobs-2.2.1.jar"/>
    <jar name="shiro-aspectj-1.2.3.jar"/>
    <jar name="shiro-core-1.2.3.jar"/>
    <jar name="shiro-ehcache-1.2.3.jar"/>
    <jar name="shiro-quartz-1.2.3.jar"/>
    <jar name="shiro-spring-1.2.3.jar"/>
    <jar name="shiro-web-1.2.3.jar"/>
    <jar name="slf4j-api-1.5.8.jar"/>
    <jar name="slf4j-nop-1.5.2.jar"/>
    <jar name="spring-aop-3.2.11.RELEASE.jar"/>
    <jar name="spring-aspects-3.2.11.RELEASE.jar"/>
    <jar name="spring-beans-3.2.11.RELEASE.jar"/>
    <jar name="spring-context-3.2.11.RELEASE.jar"/>
    <jar name="spring-context-support-3.2.11.RELEASE.jar"/>
    <jar name="spring-core-3.2.11.RELEASE.jar"/>
    <jar name="spring-data-commons-core-1.2.0.RELEASE.jar"/>
    <jar name="spring-data-jpa-1.0.2.RELEASE.jar"/>
    <jar name="spring-data-mongodb-1.0.2.RELEASE.jar"/>
    <jar name="spring-data-redis-1.1.0.RELEASE.jar"/>
    <jar name="spring-expression-3.2.11.RELEASE.jar"/>
    <jar name="spring-instrument-3.2.11.RELEASE.jar"/>
    <jar name="spring-instrument-tomcat-3.2.11.RELEASE.jar"/>
    <jar name="spring-jdbc-3.2.11.RELEASE.jar"/>
    <jar name="spring-jms-3.2.11.RELEASE.jar"/>
    <jar name="spring-orm-3.2.11.RELEASE.jar"/>
    <jar name="spring-oxm-3.2.11.RELEASE.jar"/>
    <jar name="spring-test-3.2.11.RELEASE.jar"/>
    <jar name="spring-tx-3.2.11.RELEASE.jar"/>
    <jar name="spring-web-3.2.11.RELEASE.jar"/>
    <jar name="spring-webmvc-3.2.11.RELEASE.jar"/>
    <jar name="spring-webmvc-portlet-3.2.11.RELEASE.jar"/>
    <jar name="sqlite-jdbc-3.5.7.jar"/>
    <jar name="struts2-convention-plugin-2.3.7.jar"/>
    <jar name="struts2-core-2.3.7.jar"/>
    <jar name="struts2-json-plugin-2.3.7.jar"/>
    <jar name="struts2-junit-plugin-2.3.7.jar"/>
    <jar name="struts2-spring-plugin-2.3.7.jar"/>
    <jar name="taobao-sdk-java-jichukaifang-20111205.jar"/>
    <jar name="urlrewritefilter-4.0.3.jar"/>
    <jar name="xwork-core-2.3.7.jar"/>
    <jar name="annotations-api.jar"/>
    <jar name="catalina-ant.jar"/>
    <jar name="catalina-ha.jar"/>
    <jar name="catalina-tribes.jar"/>
    <jar name="catalina.jar"/>
    <jar name="ecj-4.4.jar"/>
    <jar name="el-api.jar"/>
    <jar name="jasper-el.jar"/>
    <jar name="jasper.jar"/>
    <jar name="jsp-api.jar"/>
    <jar name="servlet-api.jar"/>
    <jar name="tomcat-api.jar"/>
    <jar name="tomcat-coyote.jar"/>
    <jar name="tomcat-dbcp.jar"/>
    <jar name="tomcat-i18n-es.jar"/>
    <jar name="tomcat-i18n-fr.jar"/>
    <jar name="tomcat-i18n-ja.jar"/>
    <jar name="tomcat-jdbc.jar"/>
    <jar name="tomcat-util.jar"/>
    <jar name="tomcat7-websocket.jar"/>
    <jar name="websocket-api.jar"/>
    </classpath>

    <keep-names>
        <class template="class com.jshop.action.backstage.interceptor.*"/>
        <class template="class * instanceof java.io.Serializable"/>
        <class access="protected+">
            <field access="protected+"/>
            <method access="protected+"/>
        </class>
    </keep-names>

    <!-- String encryption -->
    <property name="string-encryption" value="enable"/>
    <property name="string-encryption-type" value="fast"/>
    <property name="string-encryption-version" value="v4"/>

    <property name="log-file" value="log.xml"/>
	</config>

#### 3、allatori参考
http://www.allatori.com/doc.html


4、更严格的config

	<config>
    <jars>
        <jar in="shopper.jar" out="../obf-shopper.jar"/>
    </jars>

    <classpath>
    <jar name="activemq-all-5.10.0.jar"/>
    <jar name="activemq-camel-5.10.0.jar"/>
    <jar name="activemq-console-5.10.0.jar"/>
    <jar name="activemq-jaas-5.10.0.jar"/>
    <jar name="activemq-pool-5.10.0.jar"/>
    <jar name="activemq-protobuf-1.1.jar"/>
    <jar name="activemq-spring-5.10.0.jar"/>
    <jar name="activemq-web-5.10.0.jar"/>
    <jar name="activiti-bpmn-converter-5.14.jar"/>
    <jar name="activiti-bpmn-layout-5.14.jar"/>
    <jar name="activiti-bpmn-model-5.14.jar"/>
    <jar name="activiti-camel-5.14.jar"/>
    <jar name="activiti-cdi.jar"/>
    <jar name="activiti-common-rest-5.14.jar"/>
    <jar name="activiti-cxf-5.14.jar"/>
    <jar name="activiti-diagram-rest-5.14.jar"/>
    <jar name="activiti-engine-5.14.jar"/>
    <jar name="activiti-explorer-5.14.jar"/>
    <jar name="activiti-json-converter-5.14.jar"/>
    <jar name="activiti-ldap-5.14.jar"/>
    <jar name="activiti-modeler-5.14.jar"/>
    <jar name="activiti-mule-5.14.jar"/>
    <jar name="activiti-osgi-5.14.jar"/>
    <jar name="activiti-rest-5.14.jar"/>
    <jar name="activiti-simple-workflow-5.14.jar"/>
    <jar name="activiti-spring-5.14.jar"/>
    <jar name="antlr-2.7.6.jar"/>
    <jar name="aopalliance-1.0.jar"/>
    <jar name="asm-3.3.1.jar"/>
    <jar name="asm-commons-3.3.jar"/>
    <jar name="aspectj-1.7.1.jar"/>
    <jar name="aspectjrt-1.7.0.jar"/>
    <jar name="aspectjweaver-1.7.0.jar"/>
    <jar name="backport-util-concurrent.jar"/>
    <jar name="c3p0-0.9.1.2.jar"/>
    <jar name="cglib-2.2.2.jar"/>
    <jar name="cglib-nodep-2.2.jar"/>
    <jar name="com.d_project.qrcode.jar"/>
    <jar name="com.google.api.translate.jar"/>
    <jar name="commons-codec-1.4.jar"/>
    <jar name="commons-collections-3.1.jar"/>
    <jar name="commons-dbcp.jar"/>
    <jar name="commons-email-1.1.jar"/>
    <jar name="commons-fileupload-1.2.2.jar"/>
    <jar name="commons-httpclient-3.0.1.jar"/>
    <jar name="commons-io-2.0.1.jar"/>
    <jar name="commons-lang-2.4.jar"/>
    <jar name="commons-lang3-3.1.jar"/>
    <jar name="commons-logging-1.1.1.jar"/>
    <jar name="commons-pool.jar"/>
    <jar name="configdebug-1.0.jar"/>
    <jar name="dom4j-1.6.1.jar"/>
    <jar name="dwr-1.1.1.jar"/>
    <jar name="ehcache-1.5.0.jar"/>
    <jar name="fetion-java-api.jar"/>
    <jar name="freemarker-2.3.19.jar"/>
    <jar name="gson-2.2.4.jar"/>
    <jar name="hibernate-jpa-2.0-api-1.0.0.Final.jar"/>
    <jar name="hibernate-validator-4.0.0.GA.jar"/>
    <jar name="hibernate3.jar"/>
    <jar name="itext-xtra-5.2.1.jar"/>
    <jar name="itextpdf-5.2.1.jar"/>
    <jar name="jackson-all-1.7.4.jar"/>
    <jar name="javamail.jar"/>
    <jar name="javassist-3.11.0.GA.jar"/>
    <jar name="jaxen-1.1-beta-6.jar"/>
    <jar name="jdom.jar"/>
    <jar name="jedis-2.1.0.jar"/>
    <jar name="joda-time-1.6.2.jar"/>
    <jar name="json-lib-2.1.jar"/>
    <jar name="json-lib-2.4-jdk15.jar"/>
    <jar name="json_simple-1.1.jar"/>
    <jar name="jsr107cache-1.1.jar"/>
    <jar name="jta-1.1.jar"/>
    <jar name="log4j-1.2.15.jar"/>
    <jar name="mail-1.4.1.jar"/>
    <jar name="mongo-2.10.1.jar"/>
    <jar name="mongo-java-driver-2.12.1.jar"/>
    <jar name="mybatis-3.1.1.jar"/>
    <jar name="mybatis-generator-core-1.3.2.jar"/>
    <jar name="mybatis-spring-1.1.1.jar"/>
    <jar name="mysql-connector-java-5.1.5-bin.jar"/>
    <jar name="ognl-3.0.5.jar"/>
    <jar name="qrcode.jar"/>
    <jar name="Qrcode_swetake.jar"/>
    <jar name="quartz-2.2.1.jar"/>
    <jar name="quartz-jobs-2.2.1.jar"/>
    <jar name="shiro-aspectj-1.2.3.jar"/>
    <jar name="shiro-core-1.2.3.jar"/>
    <jar name="shiro-ehcache-1.2.3.jar"/>
    <jar name="shiro-quartz-1.2.3.jar"/>
    <jar name="shiro-spring-1.2.3.jar"/>
    <jar name="shiro-web-1.2.3.jar"/>
    <jar name="slf4j-api-1.5.8.jar"/>
    <jar name="slf4j-nop-1.5.2.jar"/>
    <jar name="spring-aop-3.2.11.RELEASE.jar"/>
    <jar name="spring-aspects-3.2.11.RELEASE.jar"/>
    <jar name="spring-beans-3.2.11.RELEASE.jar"/>
    <jar name="spring-context-3.2.11.RELEASE.jar"/>
    <jar name="spring-context-support-3.2.11.RELEASE.jar"/>
    <jar name="spring-core-3.2.11.RELEASE.jar"/>
    <jar name="spring-data-commons-core-1.2.0.RELEASE.jar"/>
    <jar name="spring-data-jpa-1.0.2.RELEASE.jar"/>
    <jar name="spring-data-mongodb-1.0.2.RELEASE.jar"/>
    <jar name="spring-data-redis-1.1.0.RELEASE.jar"/>
    <jar name="spring-expression-3.2.11.RELEASE.jar"/>
    <jar name="spring-instrument-3.2.11.RELEASE.jar"/>
    <jar name="spring-instrument-tomcat-3.2.11.RELEASE.jar"/>
    <jar name="spring-jdbc-3.2.11.RELEASE.jar"/>
    <jar name="spring-jms-3.2.11.RELEASE.jar"/>
    <jar name="spring-orm-3.2.11.RELEASE.jar"/>
    <jar name="spring-oxm-3.2.11.RELEASE.jar"/>
    <jar name="spring-test-3.2.11.RELEASE.jar"/>
    <jar name="spring-tx-3.2.11.RELEASE.jar"/>
    <jar name="spring-web-3.2.11.RELEASE.jar"/>
    <jar name="spring-webmvc-3.2.11.RELEASE.jar"/>
    <jar name="spring-webmvc-portlet-3.2.11.RELEASE.jar"/>
    <jar name="sqlite-jdbc-3.5.7.jar"/>
    <jar name="struts2-convention-plugin-2.3.7.jar"/>
    <jar name="struts2-core-2.3.7.jar"/>
    <jar name="struts2-json-plugin-2.3.7.jar"/>
    <jar name="struts2-junit-plugin-2.3.7.jar"/>
    <jar name="struts2-spring-plugin-2.3.7.jar"/>
    <jar name="taobao-sdk-java-jichukaifang-20111205.jar"/>
    <jar name="urlrewritefilter-4.0.3.jar"/>
    <jar name="xwork-core-2.3.7.jar"/>
    <jar name="annotations-api.jar"/>
    <jar name="catalina-ant.jar"/>
    <jar name="catalina-ha.jar"/>
    <jar name="catalina-tribes.jar"/>
    <jar name="catalina.jar"/>
    <jar name="ecj-4.4.jar"/>
    <jar name="el-api.jar"/>
    <jar name="jasper-el.jar"/>
    <jar name="jasper.jar"/>
    <jar name="jsp-api.jar"/>
    <jar name="servlet-api.jar"/>
    <jar name="tomcat-api.jar"/>
    <jar name="tomcat-coyote.jar"/>
    <jar name="tomcat-dbcp.jar"/>
    <jar name="tomcat-i18n-es.jar"/>
    <jar name="tomcat-i18n-fr.jar"/>
    <jar name="tomcat-i18n-ja.jar"/>
    <jar name="tomcat-jdbc.jar"/>
    <jar name="tomcat-util.jar"/>
    <jar name="tomcat7-websocket.jar"/>
    <jar name="websocket-api.jar"/>
    <!-- <jar name="shopperaction.jar"/>-->
    </classpath>

    <keep-names>
        <class template="class * instanceof java.io.Serializable"/>
        <class template="class com.jshop.action.*"/>
        <class access="private+">
            <field access="private+"/>
            <method access="protected+"/>
        </class>
    </keep-names>

    <!-- String encryption -->
    <property name="string-encryption" value="maximum"/>
    <!-- <property name="string-encryption-type" value="fast"/> -->
    <property name="string-encryption-type" value="strong"/>
    <property name="string-encryption-version" value="v4"/>

    <property name="log-file" value="log.xml"/>
	</config>

	


