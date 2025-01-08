# Spring拦截器,过滤器,切面

在`Spring Boot`的 Web 应用中，过滤器`Filter`、拦截器`Interceptor`和 切面`Aspect`是实现横切关注点功能的三种不同机制，主要区别在于实现的范围、灵活性和执行顺序。

### 1. 过滤器（Filter）

过滤器是 Servlet 规范中的概念，作用于 Servlet 容器中，请求和响应都可以被过滤器拦截并处理。  
   
实现方式：实现 javax.servlet.Filter 接口，重写 doFilter() 方法。
   
使用场景：通用的请求处理操作，比如字符编码设置、跨域处理、IP 限制等。
   
优点：可以处理所有请求，不仅限于 Spring MVC。
   
执行顺序：在拦截器之前执行，主要用于通用性较强的操作。执行顺序可以通过 @Order 注解或配置文件中的 filter 顺序来控制。
   
   ```java
   package javax.servlet;

   import java.io.IOException;
   
   public interface Filter {
       default public void init(FilterConfig filterConfig) throws ServletException {}
   
       public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException;
   
       default public void destroy() {}
   }
   ```

* init方法：Web 容器在启动时，会触发每个 Filter 实例的 init 方法调用并传递一个 FilterConfig 对象，该配置允许过滤器获取初始化参数以及 ServletContext 上下文对象，从而加载任何所需的资源。该方法在 Filter 的整个生命周期中仅会在初始化时被调用一次。

> 该方法如果抛出异常，Web 容器就会认为这个过滤器无法正常工作，因此不会将它加入到过滤器链中，无法提供后续的请求过滤工作。

* doFilter方法：该方法为 Filter 的核心工作方法，每一次请求都会调用该方法。

> FilterChain 接口参数由具体的 Servlet 容器实现并提供。每个过滤器的 doFilter 方法都会接收一个 FilterChain 对象作为参数。在这个方法内部，过滤器可以选择：
>   * 直接处理请求/响应。
>   * 调用 chain.doFilter(request, response) 将请求传递给下一个过滤器或目标资源。

* destroy方法：Web 容器在销毁时，会触发每个 Filter 实例的 destroy 方法调用，清理过滤器所有持有的资源（如内存、文件句柄、线程等）。该方法在 Filter 的整个生命周期中也仅会执行一次。

#### Filter 的配置使用

### 2. 拦截器（Interceptor）

拦截器主要用于对 HTTP 请求的预处理和后处理。通常用于处理控制器方法之前或之后的逻辑，比如权限验证、日志记录等。

实现方式：实现 HandlerInterceptor 接口（Spring 提供的拦截器），常用的三个方法：
preHandle()：在控制器方法执行前调用，返回 true 继续执行后续流程，返回 false 则停止执行。
postHandle()：在控制器方法执行后调用，但在视图渲染前调用。
afterCompletion()：在视图渲染后调用，用于资源清理等操作。

使用场景：请求鉴权、日志记录、数据绑定等。

优点：支持多层级请求的拦截，且与 Spring MVC 框架深度集成。

执行顺序：在 Spring MVC 的处理流程中，位于控制器方法之前和之后。
   
### 3. 切面（Aspect）

    * 切面用于 AOP（Aspect-Oriented Programming，面向切面编程）中，通过定义横切关注点，把公共代码从业务逻辑中分离出来，增强方法的逻辑。
   
    * 实现方式：使用 @Aspect 注解定义切面，结合 @Around、@Before、@After 等注解来定义切入点和增强逻辑。
   
    * 使用场景：事务管理、日志记录、异常处理、性能监控等。
   
    * 优点：AOP 支持更精细的控制，能够为特定方法甚至特定类定义增强逻辑。
   
    * 执行顺序：切面在过滤器和拦截器之后执行，进入方法之前和方法执行完毕后可自定义执行逻辑。通常可以通过 @Order 注解来调整多个切面的顺序。

执行顺序：

![image_2.png](image_2.png)