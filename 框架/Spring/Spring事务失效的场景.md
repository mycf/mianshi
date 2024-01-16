我们知道，Spring 中的 `@Transactional` 注解是基于 AOP 机制实现的，而 Spring AOP 又是基于 Java 的动态代理实现的，换句话说，如果动态代理失效，Spring 的事务也会随之失效。除此之外，Spring 的事务失效的场景还有:@Transactional 注解使用不当事务方法内异常被捕获、数据库引擎不支持事务等
