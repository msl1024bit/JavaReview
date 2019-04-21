# Spring
## Spring MVC
### ��������
Spring MVC���������������������ô��·�Ϊ�������̣�
1. `ContextLoaderListener`��ʼ����ʵ����IoC����������������ʵ��ע�ᵽ`ServletContext`�С�
2. `DispatcherServlet`��ʼ���������Լ��������ģ�Ҳע�ᵽ`ServletContext`�С�

web��������ͨ�����������òź�Spring����������������������web������`ServletContext`������ΪSpring��Ioc�����ṩ��һ���������ڽ�����Ioc������ϵ֮�󣬰�`DispatcherServlet`��ΪSpring MVC����web�����ת���������������Ӷ������ӦHttp�����׼����

**Spring IOC����������**

`ContextLoaderListener`ʵ��`ServletContextListener`������ӿ�����ĺ�������web�������������ڱ����á���Ϊ`ServletContextListener`��`ServletContext`�ļ����ߣ��ڷ�����������`ServletContext`��������ʱ��`ServletContextListener`��`contextInitialized()`���������ã��Ӷ���ʼ��ʼ��Spring IOC������
![�˴�����ͼƬ������](images/spring-mvc-contextListener-start.png)
���ȴ�Servlet�������¼��еõ�`ServletContext`��Ȼ���ȡ`web.xml`�еĸ�����ص�����ֵ������`ContextLoader`��ʵ����`WebApplicationContext`�����������ͳ�ʼ���Ĺ��̣��������ʼ���ĵ�һ����������Ϊ**��������**�����ڣ����������������󣬱��󶨵�webӦ�ó����`ServletContext`�ϣ�������IOC�����е���Ϳ������κεط����ʵ��ˡ�

**DispatchServlet������**

`DispatchServlet`��������һ��Servlet��web����������ʱ��servletҲ���ʼ������init���������ã�������ʼ��֮�á�
`DispatchServlet`�Ὠ���Լ���������������Spring MVC�����bean�����ڽ�������Լ����е�Ioc������ʱ�򣬻��`ServletContext`�еõ�����������Ϊ`DispatchServlet`�����ĵ�parent�����ġ���������������ģ��ٶ��Լ����е������Ľ��г�ʼ���������Լ����е���������ı��浽`ServletContext`�У����Ժ������ʹ�á�

![�˴�����ͼƬ������](images/spring-mvc-dispatchservlet-start.png)

��`initWebApplicationContext`������˶��Լ������ĵĳ�ʼ����������Ҳ��һ��`refresh`�Ĺ��̣�����ͨ��Ioc������ʼ����ͬС�졣

����һЩMVC�����Գ�ʼ��ʱ��`initStrategies()`��ʵ�ֵģ�����֧�ֹ��ʻ���`LocalResolver`��**֧��Requestӳ���`HandlerMappings`**���Լ���ͼ���ɵ�`ViewResolver`�ȵȡ�


### ִ������
![�˴�����ͼƬ������](images/spring-mvc-dispatchServlet-invoke-process.png)

![�˴�����ͼƬ������](images/spring-mvc-dispatchServlet-invoke-process-2.png)

## SpringBoot
### ��������
1. ����`SpringApplication`�ĳ�ʼ��ģ�飬����һЩ�����Ļ�����������Դ����������������
2. ʵ����Ӧ�þ�������������������������̵ļ���ģ�顢�������û���ģ�顢�����ĵĴ��������Ļ���ģ��
3. ���Զ�������ģ�飬��ģ����Ϊspringboot�Զ����ú���

### SpringApplication.run()������Щ�£�
- ����`SpringApplication`�����ڸö����ʼ��ʱ���ҵ����õ��¼�����������������
- ���ô����õ�`SpringApplication`�������`run()����`����ʱ�Ὣ�ղű�����¼����������ݵ�ǰʱ��������ͬ���¼�������������ʼ��������������ɵȣ�ͬʱҲ��ˢ��IoC���������������ɨ�衢���������صȹ�����

![�˴�����ͼƬ������](images/spring-boot-new-spring-application.jpeg)
![�˴�����ͼƬ������](images/spring-boot-spring-application-run.jpeg)

### ����Դ��
```
    /**
 * ����Ӧ�ó��򣬴�����ˢ��һ���µ�Ӧ�ó���������
 *
 * @param args
 * @return
 */
public ConfigurableApplicationContext run(String... args) {
	/**
	 *  StopWatch: �򵥵��������ʱ��һЩ���񣬹���ÿ��ָ�������������ʱ�������ʱ�䡣
	 *  ����������Ʋ����̰߳�ȫ�ģ�û��ʹ��ͬ����SpringApplication���ڵ��̻߳����£�ʹ�ð�ȫ��
	 */
	StopWatch stopWatch = new StopWatch();
	// ���õ�ǰ������ʱ��Ϊϵͳʱ��startTimeMillis = System.currentTimeMillis();
	stopWatch.start();
	// ����һ��Ӧ������������
	ConfigurableApplicationContext context = null;
	// �쳣�ռ������������쳣
	Collection<SpringBootExceptionReporter> exceptionReporters = new ArrayList<>();
	/**
	 * ϵͳ����headlessģʽ��һ��ȱ����ʾ�豸�����̻����Ļ����£��������������
	 * ͨ�����ԣ�java.awt.headless=true����
	 */
	configureHeadlessProperty();
	/*
	 * ��ȡ�¼����ͼ�������������¼���������֧ĳ����¼��ļ�����
	 * �¼�����ԭ��������¼�����ԭ��ͼ
	 */
	SpringApplicationRunListeners listeners = getRunListeners(args);
	/**
	 * ����һ�������¼�(ApplicationStartingEvent)��ͨ��������������֧�ִ��¼��ļ�����
	 */
	listeners.starting();
	try {
		// �ṩ����������SpringApplication�Ĳ����ķ��ʡ�ȡĬ��ʵ��
		ApplicationArguments applicationArguments = new DefaultApplicationArguments(args);
		/**
		 * ��������������������������ļ�
		 */
		ConfigurableEnvironment environment = prepareEnvironment(listeners, applicationArguments);
		// �Ի�����һЩbean��������
		configureIgnoreBeanInfo(environment);
		// ��־����̨��ӡ����
		Banner printedBanner = printBanner(environment);
		// ��������
		context = createApplicationContext();
		exceptionReporters = getSpringFactoriesInstances(SpringBootExceptionReporter.class, new Class[] { ConfigurableApplicationContext.class }, context);
		/**
		 * ׼��Ӧ�ó���������
		 * ׷��Դ��prepareContext������ȥ���ǿ��Է�������׼���׶�������������飺
		 * �����������û��������Ҽ�����������ʼ����������¼������־��
		 * ��������singleton������ӵ��˹�����singleton�����С�
		 * ��bean���ص�Ӧ�ó����������С�
		 */
		prepareContext(context, environment, listeners, applicationArguments, printedBanner);
		/**
		 * ˢ��������
		 * 1��ͬ��ˢ�£��������ĵ�bean�������������ˢ��׼��ʹ�ã���ʼ���������ĵ���ϢԴ��ע������bean�Ĵ����������������bean��ע�����ǣ�ʵ��������ʣ���(���ӳ�-init)������
		 * 2���첽����һ��ͬ���߳�ȥʱʱ��������Ƿ񱻹رգ����رմ�Ӧ�ó��������ģ�������bean�����е�����bean��
		 * �������ײ��refresh�����������϶�
		 */
		refreshContext(context);
		afterRefresh(context, applicationArguments);
		// stopwatch �����þ��Ǽ�¼�������ĵ�ʱ�䣬�Ϳ�ʼ������ʱ�����Ϣ��¼����
		stopWatch.stop();
		if (this.logStartupInfo) {
			new StartupInfoLogger(this.mainApplicationClass).logStarted(getApplicationLog(), stopWatch);
		}
		// ����һ�����������¼�
		listeners.started(context);
		callRunners(context, applicationArguments);
	}
	catch (Throwable ex) {
		handleRunFailure(context, ex, exceptionReporters, listeners);
		throw new IllegalStateException(ex);
	}
	try {
		// ����һ�������е��¼�
		listeners.running(context);
	}
	catch (Throwable ex) {
		// �����쳣������ᷢ��һ��ʧ�ܵ��¼�
		handleRunFailure(context, ex, exceptionReporters, null);
		throw new IllegalStateException(ex);
	}
	return context;
}
```

### �Զ�����ԭ��
[�Զ�����ԭ��](https://www.cnblogs.com/jiadp/p/9276826.html)

## Spring IoC
### BeanFactory��ApplicationContext������
1. ����`MessageSource`���й��ʻ�
2. ǿ����¼�����(Event)��
    `ApplicationContext`���¼�������Ҫͨ��`ApplicationEvent`��`ApplicationListener`�������ӿ����ṩ�ģ���java swing�е��¼�����һ��������`ApplicationContext`�з���һ���¼���ʱ��������չ��`ApplicationListener`��Bean��������ܵ�����¼�����������Ӧ�Ĵ���  
3. �ײ���Դ�ķ���  
    `ApplicationContext`��չ��`ResourceLoader`(��Դ������)�ӿڣ��Ӷ������������ض��Resource����`BeanFactory`��û����չ`ResourceLoader ` 
4. ��WebӦ�õ�֧��  
   ��`BeanFactory`ͨ���Ա�̵ķ�ʽ��������ͬ���ǣ�`ApplicationContext`���������ķ�ʽ��������ʹ��`ContextLoader`����Ȼ��Ҳ����ʹ��`ApplicationContext`��ʵ��֮һ���Ա�̵ķ�ʽ����`ApplicationContext`ʵ�� ��
5. ������ʽ
    `BeanFactroy`���õ����ӳټ�����ʽ��ע��Bean�ġ���`ApplicationContext`���෴����������������ʱ��һ���Դ��������е�Bean��
6. `PostProcesor`��ʹ��
    `BeanFactory`��`ApplicationContext`��֧��`BeanPostProcessor`��`BeanFactoryPostProcessor`��ʹ�ã�������֮��������ǣ�`BeanFactory`��Ҫ�ֶ�ע�ᣬ��`ApplicationContext`�����Զ�ע��

### Spring Bean��������
**ApplicationContext Bean��������**

![�˴�����ͼƬ������](images/spring-ioc-application-context-bean-life-circle.png)

**BeanFactory Bean��������**

![�˴�����ͼƬ������](images/spring-ioc-bean-factory-bean-life-circle.png)

**��������**
1. `BeanFactory`�����У��������`ApplicationContextAware`�ӿڵ�`setApplicationContext()`����
2. `BeanPostProcessor`�ӿڵ�`postProcessBeforeInitialzation()`������`postProcessAfterInitialization()`���������Զ����ã������Լ�ͨ�������ֶ�ע��
3. `BeanFactory`����������ʱ�򣬲���ȥʵ��������Bean,��������scopeΪsingleton�ҷ������ص�BeanҲ��һ���������ڵ��õ�ʱ��ȥʵ������

### ����ע��
- ���캯��ע��

```
    <bean id="userDao4Oracle" class="com.tgb.spring.dao.UserDao4OracleImpl"/>    
    <bean id="userManager" class="com.tgb.spring.manager.UserManagerImpl"> 
        <constructor-arg ref="userDao4Oracle"/>    
    </bean>    
```
    
```
    public class UserManagerImpl implements UserManager{    
        
        private UserDao userDao;    
        
        //ʹ�ù��췽ʽ��ֵ    
        public UserManagerImpl(UserDao userDao) {    
            this.userDao = userDao;    
        }    
        
        @Override    
        public void addUser(String userName, String password) {    
        
            userDao.addUser(userName, password);    
        }    
```

- setterע��
```
    <bean id="userDao4Oracle" class="com.tgb.spring.dao.UserDao4OracleImpl"/> 
    <bean id="userManager" class="com.tgb.spring.manager.UserManagerImpl">
        <property name="userDao" ref="userDao4Oracle"></property>    
    </bean>    
```

```
    public class UserManagerImpl implements UserManager{    
    
    private UserDao userDao;    
    
    //ʹ����ֵ��ʽ��ֵ    
    public void setUserDao(UserDao userDao) {    
        this.userDao = userDao;    
    }    
        
    @Override    
    public void addUser(String userName, String password) {    
    
        userDao.addUser(userName, password);    
    }    
} 
```

- ��̬����ע��
```
    public class DaoFactory { 
        //��̬���� 
        public static final FactoryDao getStaticFactoryDaoImpl(){ 
            return new StaticFacotryDaoImpl(); 
        } 
    }
    
    
    public class SpringAction { 
        //ע����� 
        private FactoryDao staticFactoryDao; 
        
        public void staticFactoryOk(){ 
            staticFactoryDao.saveFactory(); 
        } 
        //ע������set���� 
        public void setStaticFactoryDao(FactoryDao staticFactoryDao) { 
            this.staticFactoryDao = staticFactoryDao; 
        } 
    } 
    
    
    <!--����bean,���ú������spring����--> 
    <bean name="springAction" class="com.bless.springdemo.action.SpringAction" > 
    <!--(3)ʹ�þ�̬�����ķ���ע�����,��Ӧ����������ļ�(3)--> 
    <property name="staticFactoryDao" ref="staticFactoryDao"></property> 
    </property> 
    </bean> 
    <!--(3)�˴���ȡ����ķ�ʽ�Ǵӹ������л�ȡ��̬����--> 
    <bean name="staticFactoryDao" class="com.bless.springdemo.factory.DaoFactory" factory-method="getStaticFactoryDaoImpl"></bean> 
```

- ʵ������ע��
```
    public class DaoFactory { 
        //ʵ������ 
        public FactoryDao getFactoryDaoImpl(){ 
        return new FactoryDaoImpl(); 
        } 
    }
    
    public class SpringAction { 
        //ע����� 
        private FactoryDao factoryDao; 
        
        public void factoryOk(){ 
            factoryDao.saveFactory(); 
        } 
        
        public void setFactoryDao(FactoryDao factoryDao) { 
            this.factoryDao = factoryDao; 
        } 
    } 
    
    <!--����bean,���ú������spring����--> 
    <bean name="springAction" class="com.bless.springdemo.action.SpringAction"> 
    <!--(4)ʹ��ʵ�������ķ���ע�����,��Ӧ����������ļ�(4)--> 
    <property name="factoryDao" ref="factoryDao"></property> 
    </bean> 
    
    <!--(4)�˴���ȡ����ķ�ʽ�Ǵӹ������л�ȡʵ������--> 
    <bean name="daoFactory" class="com.bless.springdemo.factory.DaoFactory"></bean> 
    <bean name="factoryDao" factory-bean="daoFactory" factory-method="getFactoryDaoImpl"></bean> 
```


**����ע�뷽���Ƚ�**

���췽��ע�룺
1. ���캯�����Ա�֤һЩ��Ҫ��������beanʵ������ʱ������úã�������ΪһЩ��Ҫ������û���ṩ������һ�����õ�Bean ʵ�����
2. ���һ��������̫�࣬��ô���캯���Ĳ��������һ����Ȼ����ɶ��Խϲ�
3. ���캯����������ļ̳к���չ����Ϊ������Ҫ���ø��ิ�ӵĹ��캯��
4. ���캯��ע����ʱ�����**ѭ������**������

setter����ע�룺
���п�ѡ���Ժ͸�����Ե��ص�

### IOC�����ĳ�ʼ��
��ʼ�������������ʵ���е�`refresh()`���������

 - `ResourceLoader`���������Դ�ļ�λ�õĶ�λ
 - `BeanDefinitionReader`������ɶ�����Ϣ�Ľ�����Bean��Ϣ��ע��

ע����̾����� IOC �����ڲ�ά����һ��HashMap ������õ���`BeanDefinition`�Ĺ��̡���� HashMap �� IoC �������� bean ��Ϣ�ĳ������Ժ�� bean �Ĳ�������Χ�����HashMap ��ʵ�ֵ�.

### IOC����������ע��
1. ����ģʽ�����Ƿ��ӳټ��صĶ��󣬻���**IOC������ʼ��**��ʱ�򱻴����ҳ�ʼ����
2. �ǵ���ģʽ�������ӳټ��صĶ�����Ӧ��**��һ����������Ҫ��Bean����**��ʱ�򱻴����ҳ�ʼ����

��Ȼ��ڳ�����ͬ������Bean���󴴽�������һ���ģ����ǵ���`AbstractBeanFactory`��`getBean`������

### autowiring�Զ�װ��ʵ��ԭ��
1. ��Bean�����Ե�������`getBean`�������������Bean�ĳ�ʼ��������ע�롣
2. ������Bean�������������õ���������Bean�����ϡ�
3. ������Bean�����ƺͱ�����Bean�����ƴ洢��IoC�����ļ����С�

### Springѭ������
���壺 ѭ����������ѭ�����ã�������������Bean�໥֮��ĳ��жԷ����ȷ�CircularityA����CircularityB��CircularityB����CircularityA���γ�һ����״���ù�ϵ��

���ȣ���Ҫ��ȷ����spring��ѭ�������Ĵ��������������
1. **��������ѭ������**����������spring�Ǵ����˵ģ�ֱ ���׳�BeanCurrentlylnCreationException�쳣��
2. **����ģʽ�µ�setterѭ������**��ͨ�����������桱����ѭ��������
3. **�ǵ���ѭ������**���޷�����

- ������ѭ������
    - `this.singletonsCurrentlylnCreation.add(beanNam��`����ǰ��Ҫ������bean ��¼�ڻ�����
    - Spring ������ÿһ�����ڴ�����bean ��ʶ������һ��**��ǰ����bean��**�У��ڴ��������н�һֱ������������У��������ڴ���bean �����з����Լ��Ѿ��ڡ���ǰ����bean �ء� ��ʱ�����׳�`BeanCurrentlylnCreationException` �쳣��ʾѭ�������������ڴ�����ϵ�bean ���ӡ� ��ǰ����bean �ء����������

- setterѭ������
    - ��������singletonFactories �� �������󹤳���cache
    - ��������earlySingletonObjects ����ǰ�ع�ĵ��������Cache
    - ��һ����singletonObjects�����������cache

���ͣ�

A��������˳�ʼ���ĵ�һ�������ҽ��Լ���ǰ�ع⵽`singletonFactories`�У���ʱ���г�ʼ���ĵڶ����������Լ���������B����ʱ�ͳ���ȥget(B)������B��û�б�create��������create���̣�B�ڳ�ʼ����һ����ʱ�����Լ������˶���A�����ǳ���get(A)������һ������`singletonObjects`(�϶�û�У���ΪA��û��ʼ����ȫ)�����Զ�������`earlySingletonObjects`��Ҳû�У���������������`singletonFactories`������Aͨ��`ObjectFactory`���Լ���ǰ�ع��ˣ�����B�ܹ�ͨ��`ObjectFactory.getObject`�õ�A����(��ȻA��û�г�ʼ����ȫ�������ܱ�û�к�ѽ)��B�õ�A�����˳������˳�ʼ���׶�1��2��3����ȫ��ʼ��֮���Լ����뵽һ������`singletonObjects��`����ʱ����A�У�A��ʱ���õ�B�Ķ���˳������Լ��ĳ�ʼ���׶�2��3������AҲ����˳�ʼ������ȥ��һ������`singletonObjects��`�����Ҹ������˵��ǣ�����B�õ���A�Ķ������ã�����B����holdס��A��������˳�ʼ����


## Spring AOP
### ��̬����
ȱ������ҪΪÿ��Ŀ��������ɲ�ͬ�Ĵ�����󣬶���������ǰ����Ҫ��д�ô����ࡣ

### ��̬����
Bean���ɴ����ʱ������ÿ��Bean��ʼ��֮�������Ҫ������`AspectJAwareAdvisorAutoProxyCreator`�е�`postProcessAfterInitialization`ΪBean���ɴ���

JDK����Ҫͨ��`Proxy.newProxyInstance`���ɴ�����󣬵���`InvocationHandler`��`invoke`����ʵ������
```
    publicObject invoke(Object proxy, Method method, Object[] args) throwsThrowable {
       MethodInvocation invocation = null;
       Object oldProxy = null;
       boolean setProxyContext = false;
 
       TargetSource targetSource = this.advised.targetSource;
       Class targetClass = null;
       Object target = null;
 
       try {
           //eqauls()��������Ŀ�����δʵ�ִ˷���
           if (!this.equalsDefined && AopUtils.isEqualsMethod(method)){
                return (equals(args[0])? Boolean.TRUE : Boolean.FALSE);
           }
 
           //hashCode()��������Ŀ�����δʵ�ִ˷���
           if (!this.hashCodeDefined && AopUtils.isHashCodeMethod(method)){
                return newInteger(hashCode());
           }
 
           //Advised�ӿڻ����丸�ӿ��ж���ķ���,ֱ�ӷ������,��Ӧ��֪ͨ
           if (!this.advised.opaque &&method.getDeclaringClass().isInterface()
                    &&method.getDeclaringClass().isAssignableFrom(Advised.class)) {
                // Service invocations onProxyConfig with the proxy config...
                return AopUtils.invokeJoinpointUsingReflection(this.advised,method, args);
           }
 
           Object retVal = null;
 
           if (this.advised.exposeProxy) {
                // Make invocation available ifnecessary.
                oldProxy = AopContext.setCurrentProxy(proxy);
                setProxyContext = true;
           }
 
           //���Ŀ��������
           target = targetSource.getTarget();
           if (target != null) {
                targetClass = target.getClass();
           }
 
           //��ȡ����Ӧ�õ��˷����ϵ�Interceptor�б�
           List chain = this.advised.getInterceptorsAndDynamicInterceptionAdvice(method,targetClass);
 
           //���û�п���Ӧ�õ��˷�����֪ͨ(Interceptor)����ֱ�ӷ������ method.invoke(target, args)
           if (chain.isEmpty()) {
                retVal = AopUtils.invokeJoinpointUsingReflection(target,method, args);
           } else {
                //����MethodInvocation
                invocation = newReflectiveMethodInvocation(proxy, target, method, args, targetClass, chain);
                retVal = invocation.proceed();
           }
 
           // Massage return value if necessary.
           if (retVal != null && retVal == target &&method.getReturnType().isInstance(proxy)
                    &&!RawTargetAccess.class.isAssignableFrom(method.getDeclaringClass())) {
                // Special case: it returned"this" and the return type of the method
                // is type-compatible. Notethat we can't help if the target sets
                // a reference to itself inanother returned object.
                retVal = proxy;
           }
           return retVal;
       } finally {
           if (target != null && !targetSource.isStatic()) {
                // Must have come fromTargetSource.
               targetSource.releaseTarget(target);
           }
           if (setProxyContext) {
                // Restore old proxy.
                AopContext.setCurrentProxy(oldProxy);
           }
       }
    }
```

CGLIB:ͨ��`enhander`���ɴ��������ͨ��`MethodInterceptor`��`intercept`����ʵ�����ص���

```
    //CGLIB�ص�AOP��������  
public Object intercept(Object proxy, Method method, Object[] args, MethodProxy methodProxy) throws Throwable {  
            Object oldProxy = null;  
            boolean setProxyContext = false;  
            Class targetClass = null;  
            Object target = null;  
            try {  
                //���֪ͨ����¶�˴���  
                if (this.advised.exposeProxy) {  
                    //���ø����Ĵ������ΪҪ�����صĴ���                                          oldProxy = AopContext.setCurrentProxy(proxy);  
                    setProxyContext = true;  
                }  
                //��ȡĿ�����  
                target = getTarget();  
                if (target != null) {  
                    targetClass = target.getClass();  
                }  
                //��ȡAOP���õ�֪ͨ  
                List<Object> chain = this.advised.getInterceptorsAndDynamicInterceptionAdvice(method, targetClass);  
                Object retVal;  
                //���û������֪ͨ  
                if (chain.isEmpty() && Modifier.isPublic(method.getModifiers())) {  
                    //ֱ�ӵ���Ŀ�����ķ���  
                    retVal = methodProxy.invoke(target, args);  
                }  
                //���������֪ͨ  
                else {  
                    //ͨ��CglibMethodInvocation���������õ�֪ͨ  
                    retVal = new CglibMethodInvocation(proxy, target, method, args, targetClass, chain, methodProxy).proceed();  
                }  
                //��ȡĿ�������󷽷��Ļص����������б�Ҫ���װΪ����  
                retVal = massageReturnTypeIfNecessary(proxy, target, method, retVal);  
                return retVal;  
            }  
            finally {  
                if (target != null) {  
                    releaseTarget(target);  
                }  
                if (setProxyContext) {  
                    //�洢���ص��Ĵ���  
                    AopContext.setCurrentProxy(oldProxy);  
                }  
            }  
        }  
```

## Spring����
### �����ʵ�ַ�ʽ��
 - ���ʽ�����ֶ�commit/rollback��
 - ����ʽ����XML���ú�ע����ʽ��

### �����ִ������
���ģ�**TransactionInterceptor**

1. ���ñ�������ǿ�ķ����������������档
2. �����������ԣ����þ����`TxMgr`��������`TxStatus`��
3. �����ǰ�Ѿ������񣬽���step 5��
4. �������񴫲���Ϊ��У���Ƿ���Ҫ�׳��쳣(`��MANDATORY`)�����߹���������Ϣ(����û��������������������û����Ҫ�����������Դ)����������(`REQUIRED`, `REQUIREDS_NEW`, `NESTED`��)���ֻ��ߴ���һ��������(`SUPPORTS`, `NOT_SUPPORTED`, `NEVER`��)������step 6��
5. �������񴫲���Ϊ���׳��쳣(`NEVER`)�����߹���������Դ����Ϣ��������������Ƿ񴴽�����(`NOT_SUPPORTED`, `REQUIRES_NEW`��)�����߸����������Ƕ������(NESTED)���߼�����������(SUPPORTS, REQUIRED, MANDATORY��)��
6. ����`TxInfo`���󶨵��̡߳�
7. �ص�`MethodInterceptor`����������ǰ�ù���������ɡ�
8. ��������쳣����step 10��
9. ����`TxStatus`�Ƿ񱻱�ǻع����������Ƿ񱻱�ǻع��Ⱦ����Ƿ�Ҫ���봦��ع����߼���ֻ����ĳ���������߽磬�ſ��ܽ��������ύ/�ع���������������Ҫ�ع�������¸���������Ҫ�ع��������������Ķ���������һ������£��������������߽緢��������Ҫ�ع������׳�`UnexpectedRollbackException`�������������step 11��
10. �����쳣��������������ж��쳣�Ƿ���Ҫ���봦��ع����߼����ǽ��봦���ύ���߼��������Ҫ�ع�������Ƿ���ĳ���������߽�����Ƿ��������ع����������������Ҫ�ع���������봦���ύ�߼���ͬ��ֻ�������������߽�ſ����������������ύ������
11. �����Ƿ����쳣������ָ�`TxInfo`Ϊǰһ�������`TxInfo`�������ڵ�ջ����
