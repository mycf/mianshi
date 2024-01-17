DispatcherServlet 是 Spring MVC 中非常核心的一个组件，它本质上是一个 Servlet。在 Soring MVC 中，DispatcherServlet 类的继承关系如下图:
![image-20231206164741292](https://gitee.com/ycfan/images/raw/master/img/image-20231206164741292.png)
在 Spring MVC 启动过程中，如果 load-on-startup 属性的值设置为 1,Tomcat 会自动创建 DispatcherServlet 对象，创建完成后，会调用 init()方法。因为 DispatcherServlet 对象本身没有实现该方法，所以实际上调用的是其父类 HttpServletBean 中的init()方法，该方法最大的作用就是创建了 Spring 容器，并初始化了 Spring MVC 的九大组件，其中包括 HandlerMappings 和 HandlerAdapters 组件的初始化。

1. 如果是MultipartContent类型的request则转换request为MultipartHttpServletRequest 类型的
2. 根据request信息使用`handlerMapping`寻找对应的Handler+拦截器链
3. 根据当前的handler寻找对应的HandlerAdapter
4. 调用拦截器的preHandler方法
5. HandlerAdapter调用handler并返回视图ModelAndView
6. 视图名称转换应用于需要添加前缀后缀的情况
7. 调用所有拦截器的postHandle方法
8. 如果在Handler实例的处理中返回了view，ViewResolver解析视图
9. 渲染视图
10. 调用拦截器器完成处理的方法