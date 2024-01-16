我们知道，Spring 中的 `@Transactional` 注解是基于 AOP 机制实现的，而 Spring AOP 又是基于 Java 的动态代理实现的，换句话说，如果动态代理失效，Spring 的事务也会随之失效。除此之外，Spring 的事务失效的场景还有：`@Transactional` 注解使用不当、事务方法内异常被捕获、数据库引擎不支持事务等。



# 动态代理失效的场景
1. 同一个类中方法的调用，而导致 `@Transactional` 注解失效。这也是我们开发中最容易犯错的一种场景。
	同一个类中的方法调用，属于 this 调用，并不会使用代理对象。不过，该问题可以使用如下方式解决
2. `@Transactional` 注解应用在==非 public== 修饰的方法上。
3. `@Transactional` 注解应用在 static/final 修饰的方法上。
# @Transactional 注解使用不当的场景
1.@Transactional 注解的 propagation 事务传播行为属性设置不当。如果将事务方法的事务传播行为设置为了不支持事务的传播类型，那么该事务方法的事务将会失效。



