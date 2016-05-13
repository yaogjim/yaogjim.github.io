### 使用hibernate事件监听实现数据入出库加解密

#### 背景

**在某处项目中需要将部分数据加密后存储到数据库，并且从数据库中获取数据后自动解密并显示到页面中。**

#### 	思路： 

* 在保存或则更新数据操作执行后，更新到数据库之前，拦截保存或则更新操作，修改对象中需要加密的数据，然后再更新到数据库； 
* 在从数据库获取数据，未加载前，对获得对象的部分数据项解密。

#### 环境

	spring+hibernate3
	
#### 实现：

**修改spring配置文件，在sessionFactory中曾加**

		<property name="eventListeners">
			<map>
			<entry key="pre-insert" value-ref="hibernateEventListener" />
			<entry key="pre-update" value-ref="hibernateEventListener" />
			<entry key="post-load" value-ref="hibernateEventListener" />
			<entry key="post-update" value-ref="hibernateEventListener" />
			</map>
		</property>
		
**增加事件处理bean配置**

	<bean id="hibernateEventListener" class="com.jandar.main.baseInfo.domain.HibernateEventListener"/>
	
**具体的事件处理HibernateEventListene**

	public class HibernateEventListener implements PostLoadEventListener, PostUpdateEventListener, PreInsertEventListener,
			PreUpdateEventListener, LoadEventListener {
		// extends DefaultLoadEventListener
	
		private static final long serialVersionUID = 1L;
	
		@Override
		public void onPostLoad(PostLoadEvent event) throws HibernateException {
			System.out.println("onLoad listener called:" + event.getEntity().getClass().getName());
			//对你感兴趣的entity 做操作
			if (Enterprise.class.getName().equals(event.getEntity().getClass().getName())) {
				Enterprise entity = (Enterprise) event.getEntity();
				// entity.setMyCustomProperty(springManagedProperty);
				System.out.println("onPostLoad listener object:" + entity.getName());
			}
			if (Project.class.getName().equals(event.getEntity().getClass().getName())) {
				Project entity = (Project) event.getEntity();
				// entity.setMyCustomProperty(springManagedProperty);
				System.out.println("onPostLoad listener object:" + entity.getProjectName());
			}
	
		}
	
		@Override
		public void onPostUpdate(PostUpdateEvent event) {
			System.out.println("onPostUpdate listener called:" + event.getEntity().getClass().getName());
	
		}
	
		@Override
		public boolean onPreUpdate(PreUpdateEvent event) {
			System.out.println("onPreUpdate listener called:" + event.getEntity().getClass().getName());
			return false;
		}
	
		@Override
		public boolean onPreInsert(PreInsertEvent event) {
			System.out.println("onPreInsert listener called:" + event.getEntity().getClass().getName());
			return false;
		}
	
		@Override
		public void onLoad(LoadEvent event, LoadType loadType) throws HibernateException {
			// TODO Auto-generated method stub
			System.out.println("onLoad listener called:" + event.getEntityClassName());
		}
	
	}