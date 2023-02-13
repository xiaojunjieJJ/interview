---
typora-root-url: images
---

#### 拦截器和过滤器的区别：

1、实现原理：Filter的实现基于函数回调，而Interceptor依赖于Java反射机制；

2、使用范围：Filter是Servlet规范规定的，只能用于Web程序中，而Interceptor既可以用于Web应用，也可以用于Application、Swing等程序中；

3、规范不同：由于Filter是Servlet规范规定的，因此必须依赖Servlet容器，例如Tomcat，Interceptor不依赖于Servlet容器，是在Spring容器内的，由Spring框架支持；

4、深度/触发时机：Filter只在Servlet前后起作用（Servlet的doService方法就是在chain.doFilter方法中执行的），而Interceptor能够深入到方法前后、异常抛出前后等（越先声明的Interceptor其postHandle方法越晚执行），因此Interceptor的使用具有更大的弹性，在Spring构建的程序中要优先使用拦截器；

![1](/1.png)

5、广度：Filter可以对几乎所有请求起作用，而Interceptor只能对controller中的请求和访问static目录下资源的请求起作用；

![2](/2.png)

6、Interceptor可以获取IOC容器中的Bean，例如在某个拦截器中注入Service可以调用业务逻辑，而Filter不行；

7、调用次数：