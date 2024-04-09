1. 异常信息：jar包冲突。

![image-20230612124842915](image-20230612124842915.png)

解决方案：

使用maven依赖分析插件，删除项目中的spring-cloud-starter-dubbo

```xml
<dependency>
			<groupId>com.alibaba.cloud</groupId>
			<artifactId>spring-cloud-starter-dubbo</artifactId>
</dependency>
<!-- Spring Cloud Nacos Service Discovery -->
<dependency>
			<groupId>com.alibaba.cloud</groupId>
			<artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
</dependency>
<!-- Spring Cloud Nacos Service config -->
<dependency>
			<groupId>com.alibaba.cloud</groupId>
			<artifactId>spring-cloud-starter-alibaba-nacos-config</artifactId>
</dependency>
```

替换为：

```xml
<dependency>
			<groupId>org.apache.dubbo</groupId>
			<artifactId>dubbo-registry-nacos</artifactId>
</dependency>
<dependency>
			<groupId>com.alibaba.nacos</groupId>
			<artifactId>nacos-client</artifactId>
</dependency>
<dependency>
			<groupId>com.alibaba.boot</groupId>
			<artifactId>nacos-config-spring-boot-starter</artifactId>
</dependency>
<dependency>
			<groupId>org.apache.dubbo</groupId>
			<artifactId>dubbo-spring-boot-starter</artifactId>
</dependency>
```

异常信息如下：

![image-20230612131141889](image-20230612131141889.png)

缺少一个LandlordContext的包：

这个包是自定义的包，用于获取日志获取traceId。

包路径为：**com.alibaba.cz.landlord**

```java
package com.alibaba.cz.landlord;

/**
 * 该类是sls记录业务日志使用，请勿删除
 */
public class LandlordContext {
    public static String getCurrentTenantId() {
        return null;
    }
}
```

异常信息如下：

![image-20230612131541650](image-20230612131541650.png)

缺少dubbo Rest的配置：

配置文件中新增rest配置：

```yaml
# dubbo rest
dubbo:
  protocols:
    rest:
      name: rest
      port: 8080
      contextpath: /
      server: servlet
```

 java.lang.IllegalState**Exception**: Unsupported protocol rest in notified url: rest://XXX

```log
java.lang.IllegalStateException: Failed to load extension class (interface: interface org.apache.dubbo.rpc.Protocol, class line: org.apache.dubbo.rpc.protocol.rest.RestProtocol) in jar:file:/home/admin/app/82dc13c8-51cb-4c04-917a-e7fecbb153ef/appTc.jar!/BOOT-INF/lib/dubbo-2.7.9.jar!/META-INF/dubbo/internal/org.apache.dubbo.rpc.Protocol, cause: org/jboss/resteasy/client/jaxrs/ClientHttpEngine
```

![image-20230612132144493](image-20230612132144493.png)

缺少maven依赖：

```xml
<dependency>
			<groupId>org.jboss.resteasy</groupId>
			<artifactId>resteasy-client</artifactId>
</dependency>
```

异常信息：

![image-20230612132307035](image-20230612132307035.png)

Dubbo 注解使用不正确：

dubbo接口需要指定请求协议是dubbo 还是rest

![image-20230612132432324](image-20230612132432324.png)

> No servlet context found. If you are using server='servlet', make sure that you've configured org.apache.dubbo.remoting.http.servlet.BootstrapListener in web.xml

缺少dubbo servletContext的配置：

改配置使用配置类实现：

```java
package com.alibaba.adapter.web;

import org.apache.dubbo.remoting.http.servlet.BootstrapListener;
import org.apache.dubbo.remoting.http.servlet.DispatcherServlet;
import org.springframework.boot.web.servlet.ServletContextInitializer;
import org.springframework.context.annotation.Configuration;

import javax.servlet.ServletContext;
import javax.servlet.ServletRegistration.Dynamic;

/**
 * Web初始化配置
 */
@Configuration
public class WebInitializer implements ServletContextInitializer {

    @Override
    public void onStartup(ServletContext servletContext) {
        servletContext.addListener(new BootstrapListener());
        Dynamic dynamic = servletContext.addServlet("dispatchServlet", new DispatcherServlet());
        dynamic.setLoadOnStartup(1);
        dynamic.addMapping("/");
    }
}

```

GC 异常：

![image-20230612125555253](image-20230612125555253.png)

![image-20230612125624078](image-20230612125624078.png)

修改JVM参数采用GC：

```
-Xms4096m -Xmx4096m  -XX:+UseG1GC -Dnacos.use.endpoint.parsing.rule=false -Dnacos.use.cloud.namespace.parsing=false -Dahas.namespace=default -Dspring.profiles.active=dev2 
```

原参数为：

新生代ParNew + 老年代CMS

服务器为：2核8G

![image-20230612093525058](image-20230612093525058.png)

异常信息：

```log
lib/dubbo-2.7.9.jar!/org/apache/dubbo/remoting/RemotingException.class

lib/dubbo-remoting-api-2.7.9.jar!/org/apache/dubbo/remoting/RemotingException.class



lib/dubbo-2.7.9.jar!/org/apache/dubbo/remoting/Transporters.class

lib/dubbo-remoting-api-2.7.9.jar!/org/apache/dubbo/remoting/Transporters.class



lib/dubbo-2.7.9.jar!/org/apache/dubbo/remoting/exchange/Exchangers.class

lib/dubbo-remoting-api-2.7.9.jar!/org/apache/dubbo/remoting/exchange/Exchangers.class


lib/dubbo-common-2.7.9.jar!/org/apache/dubbo/common/Version.class

lib/dubbo-2.7.9.jar!/org/apache/dubbo/common/Version.class


```

依赖包冲突。

Dubbo.jar 中包含dubbo-remoting-api-2.7.9.jar，以及dubbo-common-2.7.9.jar。使用maven依赖分析排除到多余的dubbo-remoting-api-2.7.9.jar，以及dubbo-common-2.7.9.jar即可。



未解决警告如下⚠️：

 You specified the config center, but there's not even one single config item in it



![image-20230612125209022](image-20230612125209022.png)

> ERROR StatusLogger Log4j2 could not find a logging implementation. Please add log4j-core to the classpath. Using SimpleLogger to log to the console...





	java.lang.IllegalStateException: Could not evaluate condition on com.alibaba.cloud.dubbo.autoconfigure.DubboServiceRegistrationAutoConfiguration#defaultSpringCloudRegistryConfig due to org/apache/dubbo/config/spring/util/PropertySourcesUtils not found. Make sure your own configuration does not rely on that class. This can also happen if you are @ComponentScanning a springframework package (e.g. if you put a @ComponentScan in the default package by mistake)
	at org.springframework.boot.autoconfigure.condition.SpringBootCondition.matches(SpringBootCondition.java:53)