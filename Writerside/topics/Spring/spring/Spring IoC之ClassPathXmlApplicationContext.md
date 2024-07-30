# Spring IoC之ClassPathXmlApplicationContext

ApplicationContext 和 BeanFactory 两者都是用于加载 Bean 的，但是相比之下，ApplicationContext 提供了更多的扩展功能 ，简而言之： ApplicationContext 包含 BeanFactory 的所有功能。通常建议优先使用  ApplicationContext。

那么究竟  ApplicationContext 比 BeanFactory 多了哪些功能？首先我们来看看使用两个不同的类去加载配置文件在写法上有何不同：

```java
//使用BeanFactory方式加载XML.虽然XmlBeanFactory在Spring 3.1中被标记为不建议使用，但是不影响我们分析源码
BeanFactory bf = new XmlBeanFactory(new ClassPathResource("application_context.xml"));

//使用ApplicationContext方式加载XML.
ApplicationContext bf = new ClassPathXmlApplicationContext("application_context.xml");
复制代码
```

接下来我们就以  ClassPathXmlApplicationContext 作为切入点，对整体功能进行分析。

### ClassPathXmlApplicationContext

为了后面便于代码阅读，先给出一下 ClassPathXmlApplicationContext 这个类的继承关系 ：

![img](16f7af970de19e6b~tplv-t2oaga2asx-zoom-in-crop-mark:3024:0:0:0-20220724201447692.awebp)

可以看出该类是  ApplicationContext 和  BeanFactory 的子接口。

构造函数

```java
public ClassPathXmlApplicationContext() {
}

public ClassPathXmlApplicationContext(ApplicationContext parent) {
    super(parent);
}

public ClassPathXmlApplicationContext(String configLocation) throws BeansException {
    this(new String[]{configLocation}, true, (ApplicationContext)null);
}

public ClassPathXmlApplicationContext(String... configLocations) throws BeansException {
    this(configLocations, true, (ApplicationContext)null);
}

public ClassPathXmlApplicationContext(String[] configLocations, @Nullable ApplicationContext parent) throws BeansException {
    this(configLocations, true, parent);
}

public ClassPathXmlApplicationContext(String[] configLocations, boolean refresh) throws BeansException {
    this(configLocations, refresh, (ApplicationContext)null);
}

public ClassPathXmlApplicationContext(String[] configLocations, boolean refresh, @Nullable ApplicationContext parent) throws BeansException {
    super(parent);
    this.setConfigLocations(configLocations);
    if (refresh) {
        this.refresh();
    }

}

public ClassPathXmlApplicationContext(String path, Class<?> clazz) throws BeansException {
    this(new String[]{path}, clazz);
}

public ClassPathXmlApplicationContext(String[] paths, Class<?> clazz) throws BeansException {
    this(paths, clazz, (ApplicationContext)null);
}

public ClassPathXmlApplicationContext(String[] paths, Class<?> clazz, @Nullable ApplicationContext parent) throws BeansException {
    super(parent);
    Assert.notNull(paths, "Path array must not be null");
    Assert.notNull(clazz, "Class argument must not be null");
    this.configResources = new Resource[paths.length];

    for(int i = 0; i < paths.length; ++i) {
        this.configResources[i] = new ClassPathResource(paths[i], clazz);
    }

    this.refresh();
}

```

从上述构造函数的代码可以看出大致分为两种，构造方法A：`public ClassPathXmlApplicationContext(String[] configLocations, boolean refresh, @Nullable ApplicationContext parent)`和构造方法B： `public ClassPathXmlApplicationContext(String[] paths, Class<?> clazz, @Nullable ApplicationContext parent)`，它们的使用稍微有点区别：

```java
ApplicationContext context = new ClassPathXmlApplicationContext("application_context.xml");

//经测试，第二个参数并无指定性的作用，所以就算换一个class也可以
ApplicationContext context = new ClassPathXmlApplicationContext("/application_context.xml", User.class);
复制代码
```

这两种构造方法从用法上来讲，肯定是构造方法A更方便，在调试过程中，发现两种方法在获取资源的方式有所不同，主要提现在这一部分：

```java
public InputStream getInputStream() throws IOException {
    InputStream is;
    if (this.clazz != null) {
        is = this.clazz.getResourceAsStream(this.path);
    } else if (this.classLoader != null) {
        is = this.classLoader.getResourceAsStream(this.path);
    } else {
        is = ClassLoader.getSystemResourceAsStream(this.path);
    }

    if (is == null) {
        throw new FileNotFoundException(this.getDescription() + " cannot be opened because it does not exist");
    } else {
        return is;
    }
}

```

构造方法A获取资源是通过 ClassLoader 获取的，而构造方法B是由 Class 获取的。

接下来我们先一步一步来分析构造方法A。

#### 构造方法之configLocations

```java
public ClassPathXmlApplicationContext(String[] configLocations, boolean refresh, @Nullable ApplicationContext parent) throws BeansException {
    super(parent);
    this.setConfigLocations(configLocations);
    if (refresh) {
        this.refresh();
    }

}

```

##### 1、 super(parent)

通过代码跟随，可以发现，`super(parent)`这个方法最终是由其基类（AbstractApplicationContext）来执行，执行了 AbstractApplicationContext 的无参构造方法和 setParent(）方法。其代码如下，省略了静态代码块的定义：

```java
//FileSystemXmlApplicationContext调用父类构造方法就是该方法
public AbstractApplicationContext(ApplicationContext parent) {
    this();
    setParent(parent);
}
//具体需要执行的方法
public AbstractApplicationContext() {
    this.resourcePatternResolver = getResourcePatternResolver();
}
//获取一个Source的加载器用于读入Spring Bean 定义资源文件
protected ResourcePatternResolver getResourcePatternResolver() {
    return new PathMatchingResourcePatternResolver(this);
}
//设置双亲ioc容器
public void setParent(ApplicationContext parent) {
    this.parent = parent;
    if (parent != null) {
        Environment parentEnvironment = parent.getEnvironment();
        if (parentEnvironment instanceof ConfigurableEnvironment) {
            getEnvironment().merge((ConfigurableEnvironment) parentEnvironment);
        }
    }
}

```

截止目前， ClassPathXmlApplicationContext 已经有了两个资源加载器（一个是由  AbstractApplicationContext 继承 DefaultResourceLoader 而来 ；一个是  AbstractApplicationContext 主动创建的 PathMatchingResourcePatternResolver ）。 DefaultResourceLoader 只能加载一个特定类路径的资源 ； PathMatchingResourcePatternResolver 可以根据 Ant 风格加载多个资源。

`super(parent);`用来设置父级 ApplicationContext，这里是 null 。

```java
@Test
public void MyBean(){
    //解析application_context.xml文件 , 生成管理相应的Bean对象
    ApplicationContext context = new ClassPathXmlApplicationContext("application_context.xml");

    System.out.println(context.getParent());//输出结果null
    User user = (User) context.getBean("user");
    System.out.println(user);
}

```

##### 2、设置文件配置路径

先看下面这个案例：

```java
@Test
public void MyBean(){
    //解析application_context.xml文件 , 生成管理相应的Bean对象
    //        ApplicationContext context = new ClassPathXmlApplicationContext("application_context.xml");

    //自定义一个系统属性，名为 spring 值为配置文件全名
    System.setProperty("spring","application_context");
    //使用占位符设置配置文件路径
    ApplicationContext context = new ClassPathXmlApplicationContext("${spring}.xml");
    System.out.println(context.getParent());
    System.out.println(context.getEnvironment());
    System.out.println(context.getClassLoader());
    User user = (User) context.getBean("user");
    System.out.println(user);

}

```

执行结果为：

```java
null
StandardEnvironment {activeProfiles=[], defaultProfiles=[default], propertySources=[PropertiesPropertySource {name='systemProperties'}, SystemEnvironmentPropertySource {name='systemEnvironment'}]}
sun.misc.Launcher$AppClassLoader@18b4aac2
User{name='hresh'}
复制代码
```

上述案例的实现，就多亏了 `setConfigLocations(configLocations)`方法，在构造方法B中就无法这样使用，接下来我们就学习一下该方法。

`setConfigLocations(configLocations)`方法用来设置配置路径， 支持多个配置文件以数组方式同时传入 。 该方法的定义在类 AbstractRefreshableConfigApplicationContext中： 

```java
//处理资源定义的数组，解析Bean文件的定义路径
public void setConfigLocations(String[] locations) {
        if (locations != null) {
            Assert.noNullElements(locations, "Config locations must not be null");
            this.configLocations = new String[locations.length];
            for (int i = 0; i < locations.length; i++) {
                //解析路径
                this.configLocations[i] = resolvePath(locations[i]).trim();
            }
        }
        else {
            this.configLocations = null;
        }
    }

protected String resolvePath(String path) {
    return this.getEnvironment().resolveRequiredPlaceholders(path);
}

```

其中如果给定的路径中包含特殊符号，如${var}，那么会在方法 resolvePath 中解析系统变量并替换 。

接着分析`this.getEnvironment()`

```java
public ConfigurableEnvironment getEnvironment() {
    if (this.environment == null) {
        this.environment = this.createEnvironment();
    }

    return this.environment;
}

protected ConfigurableEnvironment createEnvironment() {
    return new StandardEnvironment();
}

```

首先 getEnvironment()获取 Environment：如果 Environment 为 null 则 new一个 StandardEnvironment 出来：`AbstractApplicationContext.createEnvironment()` 。

而 StandardEnvironment 继承于 AbstractEnvironment，AbstractEnvironment 中包含 MutablePropertySources 属性： 

```java
public class StandardEnvironment extends AbstractEnvironment {
    public static final String SYSTEM_ENVIRONMENT_PROPERTY_SOURCE_NAME = "systemEnvironment";
    public static final String SYSTEM_PROPERTIES_PROPERTY_SOURCE_NAME = "systemProperties";

    public StandardEnvironment() {
    }

    protected void customizePropertySources(MutablePropertySources propertySources) {
        propertySources.addLast(new PropertiesPropertySource("systemProperties", this.getSystemProperties()));
        propertySources.addLast(new SystemEnvironmentPropertySource("systemEnvironment", this.getSystemEnvironment()));
    }
}

public abstract class AbstractEnvironment implements ConfigurableEnvironment {
    private final MutablePropertySources propertySources = new MutablePropertySources();
    //PropertyResolver : Environment的顶层接口，主要提供属性检索和解析带占位符的文本。
    private final ConfigurablePropertyResolver propertyResolver;

    public AbstractEnvironment() {
        this.propertyResolver = new PropertySourcesPropertyResolver(this.propertySources);
        //这个抽象方法在StandardEnvironment中实现
        this.customizePropertySources(this.propertySources);
    }

    protected void customizePropertySources(MutablePropertySources propertySources) {
    }

    .....
}

```

getSystemProperties()： 

```java
public Map<String, Object> getSystemProperties() {
    try {
        return System.getProperties();
    } catch (AccessControlException var2) {
        return new ReadOnlySystemAttributesMap() {
            @Nullable
            protected String getSystemAttribute(String attributeName) {
                try {
                    return System.getProperty(attributeName);
                } catch (AccessControlException var3) {
                    if (AbstractEnvironment.this.logger.isInfoEnabled()) {
                        AbstractEnvironment.this.logger.info("Caught AccessControlException when accessing system property '" + attributeName + "'; its value will be returned [null]. Reason: " + var3.getMessage());
                    }

                    return null;
                }
            }
        };
    }
}

```

System 类中实现了  System.getProperties() ：

```java
public static Properties getProperties() {
    SecurityManager var0 = getSecurityManager();
    if (var0 != null) {
        var0.checkPropertiesAccess();
    }

    return props;
}

```

Properties 类：

```java
public class Properties extends Hashtable<Object, Object>

```

由此段代码可以看出 Properties 继承于 Hashtable，以键值对的方式实现。 

**回到上面的路径解析问题： `this.getEnvironment().resolveRequiredPlaceholders(path)`就是启动了`AbstractEnvironment.resolveRequiredPlaceholders(path)`：**

```java
public String resolveRequiredPlaceholders(String text) throws IllegalArgumentException {
    return this.propertyResolver.resolveRequiredPlaceholders(text);
}

```

进而转换为  `AbstractEnvironment.propertyResolver.resolveRequiredPlaceholders(text)` , 而 AbstractEnvironment.propertyResolver： 

```java
public AbstractEnvironment() {
    this.propertyResolver = new PropertySourcesPropertyResolver(this.propertySources);
    this.customizePropertySources(this.propertySources);
}

```

PropertySourcesPropertyResolver 继承于 AbstractPropertyResolver，则最终触发 `AbstractPropertyResolver.resolveRequiredPlaceholders(String text)`:

```java
public String resolveRequiredPlaceholders(String text) throws IllegalArgumentException {
    //初始化占位符解析器
    if (this.strictHelper == null) {
        this.strictHelper = this.createPlaceholderHelper(false);
    }

    //调用doResolvePlaceholders，进行替换占位符具体值
    return this.doResolvePlaceholders(text, this.strictHelper);
}

private String doResolvePlaceholders(String text, PropertyPlaceholderHelper helper) {
    //替换占位符具体值
    return helper.replacePlaceholders(text, this::getPropertyAsRawString);
}

```

追溯 replacePlaceholders() 方法，定位到 PropertyPlaceholderHelper 类中：

```java
public String replacePlaceholders(String value, PropertyPlaceholderHelper.PlaceholderResolver placeholderResolver) {
    Assert.notNull(value, "'value' must not be null");
    return this.parseStringValue(value, placeholderResolver, (Set)null);
}

```

查看代码一直没明白调用该方法时的参数值，`this::getPropertyAsRawString` 指的是 PropertySourcesPropertyResolver 类中的 getPropertyAsRawString 方法，返回值为 String 类型，但是进入 replacePlaceholders 方法中的却是 PlaceholderResolver 类型。在网上看到这么一段代码：

```java
private String doResolvePlaceholders(String text, PropertyPlaceholderHelper helper) {
                //替换占位符具体值
        return helper.replacePlaceholders(text, new PropertyPlaceholderHelper.PlaceholderResolver() {
            @Override
            public String resolvePlaceholder(String placeholderName) {
                                //通过key获取占位符对应的String类型具体值
                return getPropertyAsRawString(placeholderName);
            }
        });
    }

```

如果是上述代码，倒是可以理解。如果有大神知道原因，请不吝赐教。

最后执行的 PropertyPlaceholderHelper 类中的 parseStringValue 方法，至此关于配置文件路径的解析就结束了。接下来就是读取配置文件、解析、注册 Bean。

扩展：关于  PropertyResolver  占位符解析，有以下代码示例：

```java
@Test
public void PropertyResolverTest(){
    PropertySource propertySource = new MapPropertySource("source", Collections.<String, Object>singletonMap("name","hresh"));
    MutablePropertySources mutablePropertySources = new MutablePropertySources();
    mutablePropertySources.addFirst(propertySource);
    PropertyResolver propertyResolver = new PropertySourcesPropertyResolver(mutablePropertySources);

    System.out.println(propertyResolver.getProperty("name"));//hresh
    System.out.println(propertyResolver.resolveRequiredPlaceholders("name is ${name}"));//hresh
}

```

PropertyResolver 的默认实现是 PropertySourcesPropertyResolver，Environment 实际上也是委托 PropertySourcesPropertyResolver 完成 **占位符的解析和类型转换。** 类型转换又是委托 [ConversionService](https://link.juejin.cn?target=https%3A%2F%2Fwww.cnblogs.com%2Fbinarylei%2Fp%2F10263589.html) 完成的。 

关于 PropertyResolver 的更多讲解参考：[Spring PropertyResolver 占位符解析（一）API 介绍](https://link.juejin.cn?target=https%3A%2F%2Fwww.cnblogs.com%2Fbinarylei%2Fp%2F10284826.html)

##### 3、 refresh()

该方法是 Spring Bean 加载的核心，它是  ClassPathXmlApplicationContext 的父类 AbstractApplicationContext 的一个方法 ， 顾名思义，用于刷新整个Spring 上下文信息，定义了整个 Spring 上下文加载的流程。

关于此方法的解读由于篇幅过长，所以会单独写一篇文章，欢迎阅读  [Spring IoC之ApplicationContext中refresh过程](https://juejin.cn/post/6844904039348436999)

#### 构造方法之paths

对于构造方法A的代码分析结束了，接下来顺带了解一下构造方法B，看看有何差别。

```java
public ClassPathXmlApplicationContext(String[] paths, Class<?> clazz, @Nullable ApplicationContext parent) throws BeansException {
    super(parent);
    Assert.notNull(paths, "Path array must not be null");
    Assert.notNull(clazz, "Class argument must not be null");
    this.configResources = new Resource[paths.length];

    for(int i = 0; i < paths.length; ++i) {
        this.configResources[i] = new ClassPathResource(paths[i], clazz);
    }

    this.refresh();
}

```

使用方式：

```java
@Test
public void MyBean(){

    ApplicationContext context = new ClassPathXmlApplicationContext("/application_context.xml", UserDao.class);
    //System.setProperty("spring","application_context");
    //ApplicationContext context = new ClassPathXmlApplicationContext("/${spring}.xml", UserDao.class);    //调用失败
    System.out.println(context.getParent());
    System.out.println(context.getEnvironment());
    System.out.println(context.getClassLoader());
    User user = (User) context.getBean("user");
    System.out.println(user);

}

```

执行结果为：

```java
null
StandardEnvironment {activeProfiles=[], defaultProfiles=[default], propertySources=[PropertiesPropertySource {name='systemProperties'}, SystemEnvironmentPropertySource {name='systemEnvironment'}]}
sun.misc.Launcher$AppClassLoader@18b4aac2
User{name='hresh'}

```

接着我们分析在代码实现方面这两个构造方法有何区别。

首先都有 `super(parent)`方法，关于此方法的调用是一致的。构造方法A中关于设置文件路径有自己的实现方法 setConfigLocations，而构造方法B中无此代码实现。具体差别可以看上文中的使用方法，构造方法B没法使用占位符设置配置文件路径。之后关于 refresh 方法中代码，主要区别在 AbstractXmlApplicationContext 中的 loadBeanDefinitions 方法。

```java
protected void loadBeanDefinitions(XmlBeanDefinitionReader reader) throws BeansException, IOException {
    //构造方法B调用
    Resource[] configResources = this.getConfigResources();
    if (configResources != null) {
        reader.loadBeanDefinitions(configResources);
    }

    //构造方法A使用
    String[] configLocations = this.getConfigLocations();
    if (configLocations != null) {
        reader.loadBeanDefinitions(configLocations);
    }

}

```

### 总结

**Spring 是一个轻量级的控制反转（IoC）和面向切面（AOP）的开源容器（框架）**。学习使用 Spring 很长一段时间了，关于 Spring 中比较重要的 IoC 功能终于结合代码分析了一遍。相关涉及到的分支比较多，一篇文章无法讲述完，后续我会将其中比较重要的知识点再拿出来单独解析一下。