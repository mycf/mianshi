MVC 即 Model-View-Controller，是一种软件设计模式，它将应用程序分为三层:**模型层、视图层和控制器层**。它们的解释如下。
1. 模型层(Model) ：**表示应用程序的核心业务逻辑和数据**。模型层通常包含一些数据结构和逻辑规则，用于处理数据的输入、输出、更新和存储等操作。模型层并不关心数据的显示或用户的交互方式，它仅关注数据本身以及对数据的操作。
2.  视图层(View)：**表示应用程序的用户界面，用于显示模型中的数据**。视图层通常包含一些页面元素和控件，用于展示数据，并且可以与用户进行交互。它并不关注数据的处理或存储方式，仅关注如何展示数据以及如何与用户进行交互。
3.控制器层(Controler):表示应用程序的处理逻辑，用于控制模型层和视图层之间的交互。控制器层通常包含一些数据处理和动作触发等操作，用于响应用户的输入或视图的变化，并对模型层进行操作。它通过将用户的输入转化为对模型的操作，从而实现了视图层和模型层之间的解耦。
MVC 软件设计模式的核心思想是将应用程序的表示和处理分离开来，从而使得应用程序更易维护和扩展，同时还可以提高代码的可读性和可复用性。
从早期的 JSP/Servlet 到后来的 Struts，在到现在我们普遍使用的 Spring MVC，都是 MVC 软件设计模式的典型案例。

另外，应用程序中还有一种经典的三层架构:表示层、业务逻辑层和数据访问层，注意它们之间的区别。