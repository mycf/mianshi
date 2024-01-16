1. 基于 XML 配置 bean。这是最古老的使用方式，虽说这种方式简单且使用灵活，但是小项目还好，一旦项目业务复杂一点，我们则需要配置更多的 bean，非常不利于维护。随着注解定义 bean 方式的兴起，目前这种方式已经使用的很少了。代码示例如下:

2. 使用 @Component 等派生注解定义 bean。鉴于 XML 定义 bean 方式的诸多缺点，Spring 2.5 版本开始支持@Component、@Controller、@Service、@Repository 注解定义 bean，其中后面三种注解都是基于 @Component 注解派生而来的。@Component 等注解的出现给我们定义 bean 提供了极大的便利，这也是目前我们项目中使用较多的一种方式。但是，需要特别注意的是，通过 @Component 注解定义 bean 的前提是:需要先配置扫描路径。代码示例如下:
3. 使用 @Configuration + @Bean 注解的方式定义 bean。@Component 等派生注解定义 bean 虽说使用起来非常方便
但是 bean 的创建过程完全交给 Spring 容器来完成，我们没办法自己控制。Spring 3.0 版本以后，开始支持JavaConfig 的方式定义 bean。代码示例如下: