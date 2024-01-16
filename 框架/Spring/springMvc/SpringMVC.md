首先，Spring MVC 是 Spring Framework 项目中的一个模块，它是在 Servlet 的基础上进行构建的一个Web 框架，它是一个典型的 MVC 软件设计模型。

Spring MVC 的整体架构设计对传统的 MVC 架构模式做了增强和扩展，主要体现在以下几个方面
1.把传统 MVC 框架中的 Controller 控制器拆分成 两部分。了前端控制器 DispatcherServlet 和后端处理器 Hand两部分;2.把 Model 模型拆分成了业务逻辑层和数据访问房
3.在视图层，可以支持不同的视图，比如:JSP、reemark、Thymeleaf 等。
Spring MVC 包含以下几个核心组件:
1.前端控制器 DispatcherServlet。