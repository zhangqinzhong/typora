# spring-BeanFactory入门

```java
DefaultListableBeanFactory beanFactory = new DefaultListableBeanFactory();

XmlBeanDefinitionReader reader = new XmlBeanDefinitionReader(beanFactory);

reader.loadBeanDefinitions("spring.xml");

Object bean = beanFactory.getBean("threadLocal");
```

