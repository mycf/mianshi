如图，Spring 中 Bean 的创建大概可以分为以下几个阶段:
1. **实例化
	- 该阶段主要通过推断构造方法并调用，完成该 Bean 的实例化。
	- 源码可以参考 [[AbstractAutowireCapableBeanFactory#createBeanInstance]]
2. **属性填充，也就是依赖注入。**
	- 该阶段主要是找出实例中需要填充的属性并进行赋值，完成依赖注入，
	- 源码可以参考 [[AbstractAutowireCapableBeanFactory#populateBean]]。
3. Aware 接口方法回调
依赖注入完成后，Spring 会判断该对象是否实现了 BeanNameAware/BeanClassLoaderAware/BeanFactoryAware 接口。
	- 如果实现了，Spring 就会相应的调用 setBeanName()/setBeanClassLoader()/setBeanFactory()方法并传入相应的参数。
	- 源码可以参考 AbstractAutowireCapableBeanFactory # invokeAwareMethods()
4.初始化前，主要是 BeanPostProcessor 的前置处理。