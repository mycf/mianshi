#面试 
在介绍 MyBatis 之前，我们先说一下 JDBC，因为任何一个 ORM 框架都是以 JDBC 作为基石进行搭建的。

JDBC，全称 Java Database Connectivity，即 Java 数据库连接，它本质上是使用 Java 语言编写的用于数据库访问的一组类和接口，接下来让我们回顾一下使用 JDBC 访问数据库的代码片段:

使用 JDBC 访问数据库快捷、高效，但是它同样存在诸多不足，比如：
1. 操作繁琐，工作量大。每次增删改査的 SQL 操作都需要这么一大块代码段，想想都头疼!有木有?
2. SQL 语句、参数传递和结果集解析存在硬编码，不易维护;
3. 频繁的创建和释放数据库资源，造成系统资源的浪费

基于 JDBC 操作数据库的诸多不足，一系列基于 JDBC 进行封装的优秀的 ORM 框架陆续问世，比如 2001 年发布的 Hibernate 和iBatis，以及后来 Spring 家族推出的 Spring Data JPA 等，其中，iBatis 就是 MyBatis 框架的前身。接下来让我们重点看下MyBatis 框架。

MyBatis 最早是 Apache 的一个开源项目 iBatis,而 iBatis 一词正是来源于 internet 和 abatis 的组合。2010 年6月，这个项目由Apache software foundation 迁移到了 Google Code 并更名为 MvBatis;2013 年，MvBatis项目迁移到了 GitHub、截止到 2024年1月 22 号，它的最新版本已经更新到了 3.5.15 版本。GitHub 地址
MyBatis 是一个半自动的 ORM 持久层框架，其底层封装了 JDBC，支持自定义 SQL 来操作数据库。相比干上述的 JDBCMyBatis 框架有以下几大核心优势: