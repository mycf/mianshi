`org.springframework.beans.factory.support.AbstractBeanFactory#doGetBean`

![[spring三级缓存 2023-12-20 23.08.26.excalidraw]]

> [!NOTE] 为什么在循环依赖的bean 中生成未完成bean
> 比如说A依赖B，B依赖A，A先创建，A创建的时候没法判断A是否发生了循环依赖，A去getBean(B)，B创建发现要注入A（getBean(A))，这时候才能知道A发生了循环依赖


> [!NOTE] 为什么不直接使用二级缓存
> 如果对象实例化的时候直接生成代理bean放入二级缓存，相当于打破了bean的生命周期，而且大部分bean是不会产生循环依赖的

```java
		boolean earlySingletonExposure = (mbd.isSingleton() && this.allowCircularReferences &&
				isSingletonCurrentlyInCreation(beanName));
```

- `mbd.isSingleton()`: `mbd` 是指 BeanDefinition 对象，`isSingleton()` 方法用于检查该 BeanDefinition 是否声明为单例模式。如果该 BeanDefinition 声明为单例，则条件成立。
- `this.allowCircularReferences`: `this` 指的是当前的 BeanFactory 实例，`allowCircularReferences` 是一个属性或配置项，用于指示是否允许循环依赖。如果允许循环依赖，则条件成立。
- `isSingletonCurrentlyInCreation(beanName)`: `isSingletonCurrentlyInCreation(beanName)` 是一个方法，用于检查给定的 beanName 是否当前正在创建中。如果给定的 beanName 正在创建中，条件成立。

只有当上述三个条件同时满足时，`earlySingletonExposure` 才会被设置为 `true`。这意味着在满足以下条件时，早期单例暴露被认为是可行的：
- BeanDefinition 声明为单例模式。
- 允许循环依赖。
- 给定的 beanName 正在创建中。

这个条件的目的是在解决循环依赖时，允许某个正在创建中的单例 bean 提前暴露给其他正在创建的 bean，以满足循环依赖的需求。