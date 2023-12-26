
![image-20231206164741292](https://gitee.com/ycfan/images/raw/master/img/image-20231206164741292.png)

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

1. `DispatcherServlet` 接收到客户端发送的请求。
2. `DispatcherServlet` 收到请求调用`HandlerMapping`处理器映射器。
3. `HandleMapping`根据请求URL 找到对应的`handler` 以及处理器 拦截器，返回给`DispatcherServlet`
4. `DispatcherServlet` 根据`handler` 调用`HanderAdapter`处理器适配器。
5. `HandlerAdapter` 根据handler 执行处理器，也就是我们controller层写的业务逻辑，并返回一个`ModeAndView`
6. `HandlerAdapter` 返回`ModeAndView`给`DispatcherServlet`
7. `DispatcherServlet` 调用`ViewResolver` 视图解析器来 来解析`ModeAndView`
8. `ViewResolver` 解析`ModeAndView` 并返回真正的view 给`DispatcherServlet`
9. `DispatcherServlet`将得到的视图进行渲染
10. 返回给客户端响应结果。
