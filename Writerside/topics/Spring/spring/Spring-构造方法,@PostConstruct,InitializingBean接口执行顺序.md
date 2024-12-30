# Spring-构造方法,@PostConstruct,InitializingBean接口执行顺序

> 日常开发代码中可能会遇到需要在某些时间点初始化一些内容。对于此类场景，文章介绍了三种常用初始化的方式

## 构造方法

*构造方法用于创建对象时,可以初始化对象的实例变量和其他属性,为对象的状态赋初值。*

构造方法比较常用，但是如果是在spring 环境中，bean是被spring管理初始化的，如果想在生成对象时候做一些初始化操作，
而这些初始化操作又依赖于依赖注入，那么就无法在构造函数中实现。这时，可以使用@PostConstruct注解一个方法来完成初始化，

## @PostConstruct

`@PostConstruct`是Java EE规范的一部分，但在Spring中也被广泛支持。它可以应用于任何类的方法上，不需要实现特定接口。

`@PostConstruct` 在依赖注入完成后立即执行，但在Bean完全初始化之前。

## InitializingBean

实现InitializingBean接口，重写afterPropertiesSet()方法来完成初始化。

InitializingBean接口的afterPropertiesSet()方法在@PostConstruct方法执行完后执行。

### 其他方法

1.init-method属性
init-method属性：这是Spring框架提供的XML配置方式或@Bean注解的属性，允许直接指定Bean的一个初始化方法。（不常用）

Spring环境中bean的执行顺序:
1. bean的构造方法
2. 属性赋值
3. BeanPostProcessor的postProcessBeforeInitialization方法
4. @PostConstruct注解修饰的方法
5. InitializingBean接口afterPropertiesSet方法
6. Init-method方法
7. BeanPostProcessor的postProcessAfterInitialization方法

