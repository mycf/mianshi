#面试 
在介绍 MyBatis 之前，我们先说一下 JDBC，因为任何一个 ORM 框架都是以 JDBC 作为基石进行搭建的。

JDBC，全称 Java Database Connectivity，即 Java 数据库连接，它本质上是使用 Java 语言编写的用于数据库访问的一组类和接口，接下来让我们回顾一下使用 JDBC 访问数据库的代码片段:

使用 JDBC 访问数据库快捷、高效，但是它同样存在诸多不足，比如：
1. 操作繁琐，工作量大。每次增删改査的 SQL 操作都需要这么一大块代码段，想想都头疼!有木有?
2. SQL 语句、参数传递和结果集解析存在硬编码，不易维护;
3. 频繁的创建和释放数据库资源，造成系统资源的浪费

基于 JDBC 操作数据库的诸多不足，一系列基于 JDBC 进行封装的优秀的 ORM 框架陆续问世，比如 2001 年发布的 Hibernate 和iBatis，以及后来 Spring 家族推出的 Spring Data JPA 等，其中，iBatis 就是 MyBatis 框架的前身。接下来让我们重点看下MyBatis 框架。

MyBatis 最早是 Apache 的一个开源项目 iBatis，而 iBatis 一词正是来源于 internet 和 abatis 的组合。2010 年6月，这个项目由Apache software foundation 迁移到了 Google Code 并更名为 MvBatis；2013 年，MvBatis项目迁移到了 GitHub、截止到 2024年1月 22 号，它的最新版本已经更新到了 3.5.15 版本。GitHub 地址

**MyBatis 是一个半自动的 ORM 持久层框架，其底层封装了 JDBC，支持自定义 SQL 来操作数据库**。相比干上述的 JDBC MyBatis 框架有以下几大核心优势:

1. 简单易学。姑且不说 SSM 框架、Spring Boot 等整合 MyBatis 有多么简单易用，就单独使用 MyBatis来讲，一样非常简单：一个 Jar 包、一个配置文件、若干 Mapper 映射文件，完事!另外，在 MyBatis 中，SQL 和代码是分离的，所以会写 SQL 基本上就会使用 MyBatis，几乎没有多余的学习成本。
2. SQL 与程序代码不在耦合。在 MyBatis 中，SQL和代码是分离的，不像 JDBC 中全部柔和在一起！SQL 与程序代码的分离，使系统的设计更清晰、更灵活、更易维护，也更易单元测试。
3. 支持多种数据源。比如 MyBatis 中内置的 POOED、UNPOOLED 和 JNDI 数据源，当然，实际生产环境中，我们并不会直接使用这三种数据源，而是更推荐使用 Druid 或者 Hikari 数据源。这两种数据源都支持配置数据库连接池来管理数据库连接，从而避免了频繁的创建和释放数据库资源，造成系统资源的浪费。
4. 支持数据库表字段和 Java 对象属性的关系的自动映射。MBatis 作为一个 ORM 框架，它可以帮我们自动完成数据库字段和Java 对象属性关系的映射。而不像 JDBC 那样，还需要操作 ResultSet 手动去完成关系的映射。
￥MyBatis中有哪几种字段映射的方式?爹数，即使所?
、表也必然有-面试题解析我们知道，MyBatis 是一个基于 JDBC 进行封装的半自动的 ORM 持久层框架。那么，在 MyBatis 中.
耦，方便维护。WeiJavaNotes
5.支持动态 SQL。动态 SQL 是 MyBatis 的强大特性之一，通过使用动态 SQL，比如 if/where/trim/foreach 等，可以大大的减少代码的开发成本。
说说MyBatis中的动态SQL
面试题解析动态 SQL 是 MyBatis 的强大特性之一。如果没有动态 SQL，我们在开发根据不同条件拼接 SQL 的需.WeiJavaNotes
6.支持一级缓存、二级缓存，当然，也可以通过集成第三方缓存组件的方式实现缓存。