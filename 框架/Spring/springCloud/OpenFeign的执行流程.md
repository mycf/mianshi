使用OpenFeign需要加`EnableFeignClients`注解，如下代码，EnableFeignClients注解上加了`@Import(FeignClientsRegistrar.class)`
```java
@Import(FeignClientsRegistrar.class)
public @interface EnableFeignClients {
```
FeignClientsRegistrar是ImportBeanDefinitionRegistrar的实现类，直接看一下registerBeanDefinitions方法
```java
public void registerBeanDefinitions(AnnotationMetadata metadata, BeanDefinitionRegistry registry) {
	// 使用EnableFeignClients#defaultConfiguration元数据注册FeignClientSpecification.class到spring容器
	registerDefaultConfiguration(metadata, registry);
	// 遍历FeignClient注解的rpc接口，为每个rpc注册一个FeignClientFactoryBean到spring容器
	// 为每个FeignClient注解的rpc接口，注册一个对应FeignClient#configuration信息的FeignClientSpecification.class到spring容器
	registerFeignClients(metadata, registry);
}

```
注入FeignClientSpecification到spring容器，从名字可以看出来是一个FeiginCilent默认配置类，信息来自于EnableFeiginClient的注解信息
```java
private void registerDefaultConfiguration(AnnotationMetadata metadata, BeanDefinitionRegistry registry) {
	Map<String, Object> defaultAttrs = metadata.getAnnotationAttributes(EnableFeignClients.class.getName(), true);

	if (defaultAttrs != null && defaultAttrs.containsKey("defaultConfiguration")) {
		String name;
		if (metadata.hasEnclosingClass()) {
			name = "default." + metadata.getEnclosingClassName();
		}
		else {
			name = "default." + metadata.getClassName();
		}
		registerClientConfiguration(registry, name, "default", defaultAttrs.get("defaultConfiguration"));
	}
}
private void registerClientConfiguration(BeanDefinitionRegistry registry, Object name, Object className,
		Object configuration) {
	BeanDefinitionBuilder builder = BeanDefinitionBuilder.genericBeanDefinition(FeignClientSpecification.class);
	builder.addConstructorArgValue(name);
	builder.addConstructorArgValue(className);
	builder.addConstructorArgValue(configuration);
	registry.registerBeanDefinition(name + "." + FeignClientSpecification.class.getSimpleName(),
			builder.getBeanDefinition());
}

```
循环遍历FeignClient注解的所有bean，并把FeignClientFactoryBean注入到spring容器中
```java
public void registerFeignClients(AnnotationMetadata metadata, BeanDefinitionRegistry registry) {
	LinkedHashSet<BeanDefinition> candidateComponents = new LinkedHashSet<>();
	Map<String, Object> attrs = metadata.getAnnotationAttributes(EnableFeignClients.class.getName());
	final Class<?>[] clients = attrs == null ? null : (Class<?>[]) attrs.get("clients");
	if (clients == null || clients.length == 0) {
		ClassPathScanningCandidateComponentProvider scanner = getScanner();
		scanner.setResourceLoader(this.resourceLoader);
		// 设置FeignClient.class为查询条件
		scanner.addIncludeFilter(new AnnotationTypeFilter(FeignClient.class));
		Set<String> basePackages = getBasePackages(metadata);
		for (String basePackage : basePackages) {
// 查找出所有的FeignClient注解的类
			candidateComponents.addAll(scanner.findCandidateComponents(basePackage));
		}
	}
	else {
		for (Class<?> clazz : clients) {
			candidateComponents.add(new AnnotatedGenericBeanDefinition(clazz));
		}
	}

	for (BeanDefinition candidateComponent : candidateComponents) {
		if (candidateComponent instanceof AnnotatedBeanDefinition beanDefinition) {
			// verify annotated class is an interface
			AnnotationMetadata annotationMetadata = beanDefinition.getMetadata();
			Assert.isTrue(annotationMetadata.isInterface(), "@FeignClient can only be specified on an interface");

			Map<String, Object> attributes = annotationMetadata
					.getAnnotationAttributes(FeignClient.class.getCanonicalName());

			// 这里是client名称
			String name = getClientName(attributes);
			String className = annotationMetadata.getClassName();
			// 为每个FeignClient注解的类注册一个配置bean到spring容器
			registerClientConfiguration(registry, name, className, attributes.get("configuration"));

			registerFeignClient(registry, annotationMetadata, attributes);
		}
	}
}

```

```java
private void registerFeignClient(BeanDefinitionRegistry registry, AnnotationMetadata annotationMetadata,
		Map<String, Object> attributes) {
	String className = annotationMetadata.getClassName();
	if (String.valueOf(false).equals(
			environment.getProperty("spring.cloud.openfeign.lazy-attributes-resolution", String.valueOf(false)))) {
		eagerlyRegisterFeignClientBeanDefinition(className, attributes, registry);
	}
	else {
		lazilyRegisterFeignClientBeanDefinition(className, attributes, registry);
	}
}

	private void eagerlyRegisterFeignClientBeanDefinition(String className, Map<String, Object> attributes,
			BeanDefinitionRegistry registry) {
		validate(attributes);
		BeanDefinitionBuilder definition = BeanDefinitionBuilder.genericBeanDefinition(FeignClientFactoryBean.class);
		definition.addPropertyValue("url", getUrl(null, attributes));
		definition.addPropertyValue("path", getPath(null, attributes));
		String name = getName(attributes);
		definition.addPropertyValue("name", name);
		String contextId = getContextId(null, attributes);
		definition.addPropertyValue("contextId", contextId);
		definition.addPropertyValue("type", className);
		definition.addPropertyValue("dismiss404", Boolean.parseBoolean(String.valueOf(attributes.get("dismiss404"))));
		Object fallback = attributes.get("fallback");
		if (fallback != null) {
			definition.addPropertyValue("fallback",
					(fallback instanceof Class ? fallback : ClassUtils.resolveClassName(fallback.toString(), null)));
		}
		Object fallbackFactory = attributes.get("fallbackFactory");
		if (fallbackFactory != null) {
			definition.addPropertyValue("fallbackFactory", fallbackFactory instanceof Class ? fallbackFactory
					: ClassUtils.resolveClassName(fallbackFactory.toString(), null));
		}
		definition.addPropertyValue("fallbackFactory", attributes.get("fallbackFactory"));
		definition.setAutowireMode(AbstractBeanDefinition.AUTOWIRE_BY_TYPE);
		definition.addPropertyValue("refreshableClient", isClientRefreshEnabled());
		String[] qualifiers = getQualifiers(attributes);
		if (ObjectUtils.isEmpty(qualifiers)) {
			qualifiers = new String[] { contextId + "FeignClient" };
		}
		// This is done so that there's a way to retrieve qualifiers while generating AOT
		// code
		definition.addPropertyValue("qualifiers", qualifiers);
		AbstractBeanDefinition beanDefinition = definition.getBeanDefinition();
		Class<?> type = ClassUtils.resolveClassName(className, null);
		beanDefinition.setAttribute(FactoryBean.OBJECT_TYPE_ATTRIBUTE, type);
		// has a default, won't be null
		boolean primary = (Boolean) attributes.get("primary");
		beanDefinition.setPrimary(primary);
		BeanDefinitionHolder holder = new BeanDefinitionHolder(beanDefinition, className, qualifiers);
		BeanDefinitionReaderUtils.registerBeanDefinition(holder, registry);
		registerRefreshableBeanDefinition(registry, contextId, Request.Options.class, OptionsFactoryBean.class);
		registerRefreshableBeanDefinition(registry, contextId, RefreshableUrl.class, RefreshableUrlFactoryBean.class);
	}


```

`FeignClientFactoryBean` 是 `FactoryBean` 的子类，所以我们要看看下 `getObject` 方法怎么生成对象。
```java
	@Override
	public Object getObject() {
		return getTarget();
	}

	/**
	 * @param <T> the target type of the Feign client
	 * @return a {@link Feign} client created with the specified data and the context
	 * information
	 */
	@SuppressWarnings("unchecked")
	<T> T getTarget() {
		// 这个bean是通过FeignAutoConfiguration自动注入地
		FeignClientFactory feignClientFactory = beanFactory != null ? beanFactory.getBean(FeignClientFactory.class)
				: applicationContext.getBean(FeignClientFactory.class);
		Feign.Builder builder = feign(feignClientFactory);
		if (!StringUtils.hasText(url) && !isUrlAvailableInConfig(contextId)) {

			if (LOG.isInfoEnabled()) {
				LOG.info("For '" + name + "' URL not provided. Will try picking an instance via load-balancing.");
			}
			if (!name.startsWith("http://") && !name.startsWith("https://")) {
				url = "http://" + name;
			}
			else {
				url = name;
			}
			url += cleanPath();
			return (T) loadBalance(builder, feignClientFactory, new HardCodedTarget<>(type, name, url));
		}
		if (StringUtils.hasText(url) && !url.startsWith("http://") && !url.startsWith("https://")) {
			url = "http://" + url;
		}
		Client client = getOptional(feignClientFactory, Client.class);
		if (client != null) {
			if (client instanceof FeignBlockingLoadBalancerClient) {
				// not load balancing because we have a url,
				// but Spring Cloud LoadBalancer is on the classpath, so unwrap
				client = ((FeignBlockingLoadBalancerClient) client).getDelegate();
			}
			if (client instanceof RetryableFeignBlockingLoadBalancerClient) {
				// not load balancing because we have a url,
				// but Spring Cloud LoadBalancer is on the classpath, so unwrap
				client = ((RetryableFeignBlockingLoadBalancerClient) client).getDelegate();
			}
			builder.client(client);
		}

		applyBuildCustomizers(feignClientFactory, builder);

		Targeter targeter = get(feignClientFactory, Targeter.class);
		return targeter.target(this, builder, feignClientFactory, resolveTarget(feignClientFactory, contextId, url));
	}

```


 在我们使用OpenFeign作为远程调用客户端的时候 它的整体执行流程如下 一通过启动类上的at enable finch client注解 开启Feign的装配和远程代理实力的创建 在at enable分几class注解源码中 可以看到它导入一个分解clients register类 该类的作用用于扫描at分解client注解过的rpc接口 通过对at分几client注解rpc接口扫描 创建远程调用的动态代理实例 分解clients register类会进行包扫描 扫描所有包下面的at分几client注解过的接口 创建rpc接口的factory bean工厂类实例 并将这些factory并注入到spring ioc容器中 如果应用的某些地方需要注入rpc接口的实例 比如被艾特瑞士引用 spring就会通过注册的factory bean 工厂实验的get不介意的方法获取rpc接口的动态代理实验 在创建rpc接口动态代理实例时 分解会为每一个rpc接口创建一个调用处理器 也会为每个接口的每一个rtc方法创建一个方法处理器 并且将方法处理器缓存在调用处理器的dispatch映射程序中 在创建动态代理实一时 分解也会通过rbc方法的注解 为每一个rbc方法生成一个request template 请求模板实例 request template中的包含了请求的所有信息 如请求的ui请求的类型 比如get请求还是post请求请求的参数等 三发生rpc调用时 通过动态代理实例内完成远程的private的http调用 当动态代理类的方法被调用的时候 分解会根据rpc方法反射实例 从调用处理器的diss party成员中获取方法处理器 然后由method handle方法处理器开始htp请求处理 method handler会根据实际的调用参数 通过request template模板实例生成request请求实例 最后将request的请求实例交给分级点client客户端 进行进一步的完成http请求处理 四在完成远程htp调用前 需要进行客户端负载均衡的处理 在spring code微服务架构中 同一个private的微服务一般都会运行多个实例 所以说客户端的负载均衡能力其实是b选项 而不是可选项 生产环境下分解必须和日本结合一起使用 所以方法处理器method handler的客户端 client的成员必须具备负载均衡能力 load balance fig client的类型 而不是完成htp请求提交的阿帕奇htp client的类型 只有在负载均衡计算出最佳的private的实力之后 才能开始http请求的提交 在load balance分解client内部有一个叫delegate的委托成员 其类型可能为负极点client default或者阿帕奇htp client ok http client等 最终由delegate客户端委托成员完成htp请求的提交 