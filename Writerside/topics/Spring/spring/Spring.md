# Spring源码深度解析

> Spring源码深度解析 读书整理

Spring是于2003年兴起的一个轻量级的Java开源框架，由Rod Johnson在其著作《Expert One-On-One J2EE Development and Design》中阐述的部分理念和原型衍生而来。Spring是为了解决企业应用开发的复杂性而创建的，它使用基本的JavaBean来完成以前只可能由EJB完成的事情。然而，Spring的用途不仅限于服务器端的开发。从简单性、可测试性和松耦合的角度而言，任何Java应用都可以从Spring中受益

## Spring的整体架构 {id="spring_1"}

Spring框架是一个分层架构，它包含一系列的功能要素，并被分为大约20个模块，如图1-1所示。

<img src="spring架构图.jpeg" alt="spring架构图" style="zoom:100%;" />

Spring官网中对IOC的定义。

### Introduction to the Spring IoC Container and Beans

This chapter covers the Spring Framework implementation of the Inversion of Control (IoC) principle.

本章介绍控制反转（IoC）原理的Spring框架实现。

>  IoC is also known as dependency injection (DI). 
> 
> IoC也称为依赖注入（DI）。
> 
> It is a process whereby objects define their dependencies (that is, the other objects they work with) only through constructor arguments, arguments to a factory method, or properties that are set on the object instance after it is constructed or returned from a factory method. 
> 
> 它是这样一个过程：对象仅通过构造函数参数、工厂方法的参数或在对象实例构造或从工厂方法返回后在对象实例上设置的属性来定义它们的依赖关系（即它们使用的其他对象）。
> 
> The container then injects those dependencies when it creates the bean. 
> 
> 然后容器在创建bean时注入这些依赖项。
> 
> This process is fundamentally the inverse (hence the name, Inversion of Control) of the bean itself controlling the instantiation or location of its dependencies by using direct construction of classes or a mechanism such as the Service Locator pattern.
> 
> 这个过程基本上是bean本身的逆过程（因此称为控制反转），通过使用类的直接构造或服务定位器模式等机制来控制其依赖项的实例化或位置。

1. 定义规范，解析各种配置文件。关键类 BeanDefinitionReader
2. 解析好之后，BeanDefinition接口，根据规则
3. 实例化：在堆中开辟空间，属性都是默认值。初始化：给属性完成赋值。
4. BeanFactory定义创建bean的规范。

java通俗理解下对象的创建方式：

1. .java文件被编译为.class文件
2. 需要被初始化时（比如new 反射等），class文件被虚拟机通过类加载器加载到jvm
3. 初始化class对象的实例供使用

![preview](v2-b785bf02600169adb09738b87d092afc_r-20211201110043617.jpg)

java 反射构建对象的方式。

1. Class.forName("完全限定名");
2. 对象.getClass();
3. 类.class;

   > Constructor ctor = clazz.getDeclareConstructor();

   > Object obj = ctor.getNewInstance();

### BeanDefinition

BeanDefinition 表示bean定义

Spring根据 BeanDefinition来创建Bean对象, BeanDefinition有很多的属性用来描述Bean

BeanDefinition中的属性：

* beanClass 表示一个bean的类型，比如OrderService.class,UserService.class,spring在创建bean的时候会根据此属性实例化得到对象。
* scope 表示一个bean的作用域,比如: scope:等于 singleton,该bean就是一个单例Bean; sope等于 prototype,该bean就是一个原型bean
* isLazy 表示一个bean是不是需要懒加載，原型bean的 isLazy属性不起作用， 懒加载的单例bean，会在第一次 getBean的时候生成该bean，非懒加载的单例bean，则会在 Spring启动过程中直接生成好。
* dependsOn 表示一个bean在创建之前依赖的其他bean，在一个bean创建之前，依赖的其他bean都要创建好。
* primary 表示一个bean是主bean，在spring中一个类型可以有多个bean对象，在进行依赖注入时，如果根据类型找到了多个bean，此时会判断是否存在一个主bean，如果存在，则直接将主bean注入给属性。
* initMethodName 表示一个bean的初始化方法，一个bean的声明周期过程中有一个步骤叫初始化，Spring会在这个步骤中去调用bean的初始化方法，初始化方法的逻辑由程序员自己控制，表示程序员可以自定义逻辑对bean进行加工。

@Component，@Bean，XML的bean标签。这些都会解析为BeanDefinition

### Spring三级缓存的执行细节

都知道spring解决循环依赖问题是通过三级缓存模式；

之所以把spring初始化缓存分为三级，主要是根据spring源码中获取bean的顺序来分的，spring获取bean的代码如下：

org.springframework.beans.factory.support.DefaultSingletonBeanRegistry

```java
  /** Cache of singleton objects: bean name to bean instance.
	 * 单例对象缓存：bean名到bean实例。
	 * */
	private final Map<String, Object> singletonObjects = new ConcurrentHashMap<>(256);

	/** 单例工厂缓存：ObjectFactory的bean名称。 Cache of singleton factories: bean name to ObjectFactory. */
	private final Map<String, ObjectFactory<?>> singletonFactories = new HashMap<>(16);

	/** Cache of early singleton objects: bean name to bean instance. */
	private final Map<String, Object> earlySingletonObjects = new ConcurrentHashMap<>(16);
 
 
@Nullable
	protected Object getSingleton(String beanName, boolean allowEarlyReference) {
		//快速检查没有完整单例锁的现有实例 Quick check for existing instance without full singleton lock
		//从单例缓存map中获取单例
		Object singletonObject = this.singletonObjects.get(beanName);
		//判断是否为空 并且 正在被创建 isSingletonCurrentlyInCreation函数返回bean是否在创建中
		if (singletonObject == null && isSingletonCurrentlyInCreation(beanName)) {
			//从早期单例对象的缓存中获取bean
			singletonObject = this.earlySingletonObjects.get(beanName);
			// 早期单例对象缓存中没拿到 并且 允许提前暴露
			if (singletonObject == null && allowEarlyReference) {
				//加锁
				synchronized (this.singletonObjects) {
					//在完整单例锁中一致地创建早期引用 Consistent creation of early reference within full singleton lock
					//dcl （double checked locking）双重检验锁
					singletonObject = this.singletonObjects.get(beanName);
					if (singletonObject == null) {
						singletonObject = this.earlySingletonObjects.get(beanName);
						if (singletonObject == null) {
							ObjectFactory<?> singletonFactory = this.singletonFactories.get(beanName);
							if (singletonFactory != null) {
								singletonObject = singletonFactory.getObject();
								this.earlySingletonObjects.put(beanName, singletonObject);
								this.singletonFactories.remove(beanName);
							}
						}
					}
				}
			}
		}
		return singletonObject;
	}
```

* 一级缓存：Map<String, Object> singletonObjects，存储可以用的bean，即：初始化完成的成品bean；
* 二级缓存：Map<String, Object> earlySingletonObjects，存储bean的实例化对象，该对象还没有完全初始化好的半成本；
* 三级缓存：Map<String, ObjectFactory<?>> singletonFactories，存储生成bean的工厂，在创建bean阶段；

### 三级缓存的特点：
* 一二级缓存保存的是bean的对象，不管是已初始化好的还是未初始化好的，这两个缓存是互斥的，bean不能既在一级缓存又在二级缓存；
* 所有的bean在创建过程中，都会用到第三级缓存，因为bean对象就是通过beanFactory创建出来的；

* 所有的bean最终都会落到第一级缓存；
* 循环依赖，先初始化谁，谁就会入第二级缓存；

### 大体步骤如下：

* 创建A bean
* 把A beanFactory放进三级缓存
* 给A bean注入B bean
* 发现B bean不存在，创建B bean
* 把B beanFactory放进三级缓存
* 给B bean注入A bean
* 从三级缓存获取到A bean，把A bean放进二级缓存，同时删除三级缓存中的A beanFactory
* 把A bean注入B bean
* B初始化完成，调用addSingleton把B bean放进一级缓存，同时删除二级缓存中的B bean和三级缓存中的B beanFactory（注意：二级缓存中没有B bean，只有A bean）
* 把B bean注入到A bean
* A初始化完成，调用addSingleton把A bean放进一级缓存，同时删除二级缓存中的A bean和三级缓存中的A beanFactory（注意：三级缓存中已没有A beanFactory）

如图：循环依赖在初始化过程中，一共有8次缓存操作：

- 三级缓存2次put，2次remove；
- 二级缓存1次put，1次remove；（都是A bean，循环依赖先初始化谁，谁就会入二级缓存）
- 一级缓存2次put；

![img](watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xzdW53aW5n,size_16,color_FFFFFF,t_70.png)



### Bean的生命周期

1. BeanDefinition定义 
2. 构造方法推断。一个方法中可能有多个构造方法，具体使用哪个构造方法创建bean对象是很复杂的。
3. 实例化具体的对象。通过构造方法反射得到一个对象，在spring中可以通过BeanPostProcessor机制对实例化进行干预。
4. 属性填充。实例化所得到的对象，是“不完整”的对象，“不完整”的意思是该对象中的某些属性还没有进行属性填充，也就是 Spring还没有自动给某些属性赋值，属性填充就是我们通常说的自动注入、依赖注入。
5. 初始化。在一个对象的属性填充之后， Spring提供了初始化机制，程序员可以利用初始化机制对Bean进行自定义加工，比如可以利用initializingBean接口来对Bean中的其他属性进行赋值，或对Bean中的某些属性进行校验。
6. 初始化后是Bean创建生命周期中最后一个步骤，我们常说的AOP机制，就是在这个步骤中通过 BeanPostProcessor机制实现的，初始化之后得到的对象才是真正的Bean对象。
7. 

##### BeanFactoryPostProcessor 增强器（后置处理器）

Factory hook that allows for custom modification of an application context's bean definitions, adapting the bean property values of the context's underlying bean factory.

工厂钩子，允许自定义修改应用程序上下文的bean定义，调整上下文基础bean工厂的bean属性值。



PlaceholderConfigurerSupport  写法为：${...}

Abstract base class for property resource configurers that resolve placeholders in bean definition property values. Implementations pull values from a properties file or other property source into bean definitions.

解析bean定义属性值中占位符的属性资源配置器的抽象基类。实现将属性文件或其他属性源中的值拉入bean定义。

BeanPostProcessor 实现AOP的基类 

该接口有两个默认方法。分别是：

1. postProcessBeforeInitialization
2. postProcessAfterInitialization

<img src="BeanFactory.png" alt="BeanFactory" style="zoom:50%;" />

Environment 接口 继承PropertyResolver

Interface representing the environment in which the current application is running. Models two key aspects of the application environment: profiles and properties. Methods related to property access are exposed via the PropertyResolver superinterface.

表示当前应用程序正在其中运行的环境的接口。为应用程序环境的两个关键方面建模：概要文件和属性。与属性访问相关的方法通过PropertyResolver超级接口公开。

实现类StandardEnvironment 获取系统级别环境变量

Environment implementation suitable for use in 'standard' (i.e. non-web) applications.
In addition to the usual functions of a ConfigurableEnvironment such as property resolution and profile-related operations, this implementation configures two default property sources, to be searched in the following order:



AbstractApplicationContext

Abstract implementation of the ApplicationContext interface. Doesn't mandate the type of storage used for configuration; simply implements common context functionality. Uses the Template Method design pattern, requiring concrete subclasses to implement abstract methods.

ApplicationContext接口的抽象实现。不强制用于配置的存储类型；简单地实现公共上下文功能。使用模板方法设计模式，需要具体的子类来实现抽象方法。

关键方法：refresh()

```java
@Override
	public void refresh() throws BeansException, IllegalStateException {
		synchronized (this.startupShutdownMonitor) {
			// Prepare this context for refreshing.
      //早期的准备工作 初始化开始时间，打开是否开启的标志 创建系统环境变量，验证是否可以解析
			prepareRefresh();

			// Tell the subclass to refresh the internal bean factory.
      //获取最新的BeanFactory工厂
      //关键方法 new DefaultListableBeanFactory ，loadBeanDefinitions加载定义的bean
			ConfigurableListableBeanFactory beanFactory = obtainFreshBeanFactory();

			// Prepare the bean factory for use in this context.
      //准备BeanFactory 以便在上下文中使用
			prepareBeanFactory(beanFactory);

			try {
				// Allows post-processing of the bean factory in context subclasses.
        //bean工厂增强器，空方法。（SpringBoot中有实现）
				postProcessBeanFactory(beanFactory);

				// Invoke factory processors registered as beans in the context.
				invokeBeanFactoryPostProcessors(beanFactory);

				// Register bean processors that intercept bean creation.
				registerBeanPostProcessors(beanFactory);

				// Initialize message source for this context.
				initMessageSource();

				// Initialize event multicaster for this context.
        //初始化时间多波器
				initApplicationEventMulticaster();

				// Initialize other special beans in specific context subclasses.
				onRefresh();

				// Check for listener beans and register them.
        //注册监听器
				registerListeners();

				// Instantiate all remaining (non-lazy-init) singletons.
        //加载所有非懒加载的单例bean
				finishBeanFactoryInitialization(beanFactory);

				// Last step: publish corresponding event.
				finishRefresh();
			}

			catch (BeansException ex) {
				if (logger.isWarnEnabled()) {
					logger.warn("Exception encountered during context initialization - " +
							"cancelling refresh attempt: " + ex);
				}

				// Destroy already created singletons to avoid dangling resources.
				destroyBeans();

				// Reset 'active' flag.
				cancelRefresh(ex);

				// Propagate exception to caller.
				throw ex;
			}

			finally {
				// Reset common introspection caches in Spring's core, since we
				// might not ever need metadata for singleton beans anymore...
				resetCommonCaches();
			}
		}
	}
```



```java
/**
	 * Prepare this context for refreshing, setting its startup date and
	 * active flag as well as performing any initialization of property sources.
	 */
	protected void prepareRefresh() {
		// Switch to active.
		this.startupDate = System.currentTimeMillis();
		this.closed.set(false);
		this.active.set(true);

		if (logger.isDebugEnabled()) {
			if (logger.isTraceEnabled()) {
				logger.trace("Refreshing " + this);
			}
			else {
				logger.debug("Refreshing " + getDisplayName());
			}
		}

		// Initialize any placeholder property sources in the context environment.
    //此方法是个空方法 没有任何实现
		initPropertySources();

		// Validate that all properties marked as required are resolvable:
		// see ConfigurablePropertyResolver#setRequiredProperties
    //getEnvironment 调用了 createEnvironment()方法，该方法new StandardEnvironment();创建了一个标准环境变量。validateRequiredProperties()验证setRequiredProperties指定的每个属性是否存在并解析
		getEnvironment().validateRequiredProperties();

		// Store pre-refresh ApplicationListeners...
    //初始化应用程序早期监听器
		if (this.earlyApplicationListeners == null) {
			this.earlyApplicationListeners = new LinkedHashSet<>(this.applicationListeners);
		}
		else {
			// Reset local application listeners to pre-refresh state.
			this.applicationListeners.clear();
			this.applicationListeners.addAll(this.earlyApplicationListeners);
		}

		// Allow for the collection of early ApplicationEvents,
		// to be published once the multicaster is available...
    //初始化 early ApplicationEvents
		this.earlyApplicationEvents = new LinkedHashSet<>();
	}
```



### FactoryBean

```java
public interface FactoryBean<T> {

	/**
	 * The name of an attribute that can be
	 * {@link org.springframework.core.AttributeAccessor#setAttribute set} on a
	 * {@link org.springframework.beans.factory.config.BeanDefinition} so that
	 * factory beans can signal their object type when it can't be deduced from
	 * the factory bean class.
	 * @since 5.2
	 */
	String OBJECT_TYPE_ATTRIBUTE = "factoryBeanObjectType";


	/**
	 * Return an instance (possibly shared or independent) of the object
	 * managed by this factory.
	 * <p>As with a {@link BeanFactory}, this allows support for both the
	 * Singleton and Prototype design pattern.
	 * <p>If this FactoryBean is not fully initialized yet at the time of
	 * the call (for example because it is involved in a circular reference),
	 * throw a corresponding {@link FactoryBeanNotInitializedException}.
	 * <p>As of Spring 2.0, FactoryBeans are allowed to return {@code null}
	 * objects. The factory will consider this as normal value to be used; it
	 * will not throw a FactoryBeanNotInitializedException in this case anymore.
	 * FactoryBean implementations are encouraged to throw
	 * FactoryBeanNotInitializedException themselves now, as appropriate.
	 * @return an instance of the bean (can be {@code null})
	 * @throws Exception in case of creation errors
	 * @see FactoryBeanNotInitializedException
	 */
	@Nullable
	T getObject() throws Exception;

	/**
	 * Return the type of object that this FactoryBean creates,
	 * or {@code null} if not known in advance.
	 * <p>This allows one to check for specific types of beans without
	 * instantiating objects, for example on autowiring.
	 * <p>In the case of implementations that are creating a singleton object,
	 * this method should try to avoid singleton creation as far as possible;
	 * it should rather estimate the type in advance.
	 * For prototypes, returning a meaningful type here is advisable too.
	 * <p>This method can be called <i>before</i> this FactoryBean has
	 * been fully initialized. It must not rely on state created during
	 * initialization; of course, it can still use such state if available.
	 * <p><b>NOTE:</b> Autowiring will simply ignore FactoryBeans that return
	 * {@code null} here. Therefore it is highly recommended to implement
	 * this method properly, using the current state of the FactoryBean.
	 * @return the type of object that this FactoryBean creates,
	 * or {@code null} if not known at the time of the call
	 * @see ListableBeanFactory#getBeansOfType
	 */
	@Nullable
	Class<?> getObjectType();

	/**
	 * Is the object managed by this factory a singleton? That is,
	 * will {@link #getObject()} always return the same object
	 * (a reference that can be cached)?
	 * <p><b>NOTE:</b> If a FactoryBean indicates to hold a singleton object,
	 * the object returned from {@code getObject()} might get cached
	 * by the owning BeanFactory. Hence, do not return {@code true}
	 * unless the FactoryBean always exposes the same reference.
	 * <p>The singleton status of the FactoryBean itself will generally
	 * be provided by the owning BeanFactory; usually, it has to be
	 * defined as singleton there.
	 * <p><b>NOTE:</b> This method returning {@code false} does not
	 * necessarily indicate that returned objects are independent instances.
	 * An implementation of the extended {@link SmartFactoryBean} interface
	 * may explicitly indicate independent instances through its
	 * {@link SmartFactoryBean#isPrototype()} method. Plain {@link FactoryBean}
	 * implementations which do not implement this extended interface are
	 * simply assumed to always return independent instances if the
	 * {@code isSingleton()} implementation returns {@code false}.
	 * <p>The default implementation returns {@code true}, since a
	 * {@code FactoryBean} typically manages a singleton instance.
	 * @return whether the exposed object is a singleton
	 * @see #getObject()
	 * @see SmartFactoryBean#isPrototype()
	 */
	default boolean isSingleton() {
		return true;
	}

}
```

