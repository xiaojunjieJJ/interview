#### 拦截器和过滤器的区别：

1、实现原理：Filter的实现基于函数回调，而Interceptor依赖于Java反射机制；

2、使用范围：Filter是Servlet规范规定的，只能用于Web程序中，而Interceptor既可以用于Web应用，也可以用于Application、Swing等程序中；

3、规范不同：由于Filter是Servlet规范规定的，是实现javax.servlet.Filter接口，因此必须依赖Servlet容器，例如Tomcat，Interceptor不依赖于Servlet容器，是在Spring容器内的，由Spring框架支持，实现HandlerInterceptor接口；

4、深度/触发时机：Filter只在Servlet前后起作用（Servlet的doService方法就是在chain.doFilter方法中执行的），而Interceptor能够深入到方法前后、异常抛出前后等（越先声明的Interceptor其postHandle方法越晚执行），因此Interceptor的使用具有更大的弹性，在Spring构建的程序中要优先使用拦截器；

5、广度：Filter可以对几乎所有请求起作用，而Interceptor只能对controller中的请求和访问static目录下资源的请求起作用；

6、Interceptor可以获取IOC容器中的Bean，例如在某个拦截器中注入Service可以调用业务逻辑，而Filter不行；

7、调用次数：Filter只能在容器初始化时被调用一次，interceptor可以被多次调用。

​		Interceptor在controller前后执行，在找到具体controller之前是DispatcherServlet的doDispatch方法中，doDispatch方法又是在doService方法中调用，doService方法又是在chain.doFilter方法中调用，因此从小到大是controller->interceptor->servlet->filter。

#### 数据库读问题：

1、脏读：事务A读事务B未提交的数据，如果事务B继续修改该数据或者回滚都会造成数据不一致；

2、不可重复读：指一个事务多次读取同一行数据结果不相同，例如事务A执行中先读了一次，事务B修改了数据并且提交，事务A再去读就不一样了；

3、幻读：一个事务读取到了别的事务插入的数据，造成前后读取的数据总量不相同。

#### #{}和${}传参各自的优缺点：

#{}：底层是PreparedStatement对Sql语句进行预编译，一个#{}就是一个?占位符，并且将传入的参数都当作字符串，自动地加双引号