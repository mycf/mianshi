

### **1.2、实现原理解析**

Spring AOP的实现原理是基于动态织入的动态代理技术，而动态代理技术又分为Java JDK动态代理和CGLIB动态代理。具体使用哪一种需要根据 AopProxyFactory 接口的 createProxy 方法中的 AdvisedSupport 中的参数进行确定，默认情况下如果目标类是接口，则使用 jdk 动态代理技术，如果是非接口类，则使用 cglib 来生成代理。具体的源码就不给大家一一贴出来了，大家可以去网上搜索一些资料进行深入了解。这里只给大家贴一张生成代理对象的图：

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f7b47c96108345efa6eafc9f9637df27~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp)

#### **1.2.1、jdk动态代理**

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/852ea0f71d224b8285ba21d91eb5bee0~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp)

spring-aop中jdk代理的实现和我们平时自己实现动态代理开发是一致的，所以此处不做详细的介绍，只是简要给大家说明一下。使用做jdk动态代理时，代理类要实现InvocationHandler接口，目标类要实现接口，因为JDK提供的Proxy类将通过目标对象的类加载器ClassLoader和Interface，以及句柄(Callback)创建与目标类拥有相同接口的代理对象proxy，该代理对象将拥有目标类接口中的所有方法，同时代理类必须实现一个类似回调函数的InvocationHandler接口并重写该接口中的invoke方法，当调用proxy的每个方法(如案例中的proxy#execute())时，invoke方法将被调用，利用该特性，可以在invoke方法中对目标对象方法执行的前后动态添加其他外围业务操作，此时无需触及目标对象的任何代码，也就实现了外围业务的操作与目标对象完全解耦合的目的。当然缺点也很明显需要拥有接口，这也就有了后来的CGLIB动态代理了。

#### **1.2.2、cglib动态代理**

spring-aop中cglib代理的实现给大家贴一下源码的路径org.springframework.aop.framework.CglibAopProxy、org.springframework.aop.framework.CglibAopProxy#getProxy(java.lang.ClassLoader)，感兴趣的话大家可以自己去看一下源码，接下来我会举一个具体的示例，来看一下如何使用cglib创建代理对象。（注：cglibgithub地址：[github.com/cglib/cglib…](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fcglib%2Fcglib%25EF%25BC%2589 "https://github.com/cglib/cglib%EF%BC%89")

##### **1.2.2.1、引入jar包**

引入jar包
```groovy
implementation 'cglib:cglib:3.3.0'
```
##### **1.2.2.2、示例代码**

```java
/**
 * 创建目标对象
 */
public class Target {
    public void execute(){
        System.out.println("执行Target的execute方法...");
    }
}

/**
 * 创建cglib代理类
 */
public class CGLibProxy implements MethodInterceptor {

    /**
     * 被代理的目标类
     */
    private Target target;

    public CGLibProxy(Target target) {
        super();
        this.target = target;
    }

    /**
     * 创建代理对象
     * @return
     */
    public Target createProxy(){
        // 使用CGLIB生成代理:
        // 1.声明增强类实例,用于生产代理类
        Enhancer enhancer = new Enhancer();
        // 2.设置被代理类字节码，CGLIB根据字节码生成被代理类的子类
        enhancer.setSuperclass(target.getClass());
        // 3.//设置回调函数，即一个方法拦截
        enhancer.setCallback(this);
        // 4.创建代理:
        return (Target) enhancer.create();
    }

    @Override
    public Object intercept(Object proxy, Method method, Object[] args, MethodProxy methodProxy) throws Throwable {
        // 指定要执行被代理的方法
        if("execute".equals(method.getName())) {
            //调用前执行方法
            System.out.println("调用前执行方法");
            //调用目标对象的方法(执行A对象即被代理对象的execute方法)
            Object result = methodProxy.invokeSuper(proxy, args);
            //记录日志数据(动态添加其他要执行业务)
            System.out.println("调用前执行方法");
            return result;
        }
        //如果不需要增强直接执行原方法
        return methodProxy.invokeSuper(proxy, args);
    }
}

/**
 * 创建测试类
 */
public class CglibTest {

    public static void main(String[] args) {
        // 将cglib生成的代理类写入到磁盘
        System.setProperty(DebuggingClassWriter.DEBUG_LOCATION_PROPERTY,"D:\test\test");
        CGLibProxy cgLibProxy = new CGLibProxy(new Target());
        Target proxy = cgLibProxy.createProxy();
        proxy.execute();
    }
}

```
执行结果

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/5af8a0f07f2c49c09f6c5a64655561fc~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp)

从代码看被代理的类无需接口即可实现动态代理，而CGLibProxy代理类需要实现一个方法拦截器接口**MethodInterceptor**并重写**intercept**方法，类似JDK动态代理的InvocationHandler接口，也是理解为回调函数，同理每次调用代理对象的方法时，intercept方法都会被调用，利用该方法便可以在运行时对方法执行前后进行动态增强。关于代理对象创建则通过**Enhancer**类来设置的，Enhancer是一个用于产生代理对象的类，作用类似JDK的Proxy类，因为CGLib底层是通过继承实现的动态代理，因此需要传递目标对象的Class，同时需要设置一个回调函数对调用方法进行拦截并进行相应处理，最后通过create()创建目标对象的代理对象。

##### **1.2.2.3、原理解析**

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/48567382c05247369397c69d67ce7db2~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp)

上图是我们在CglibTest 的main方法中设置了将代理类写入到磁盘之后生成的文件，一共有三个。我们这里重点看一下第二个文件，下面是该class文件的源码，给大家贴一下，一些重要的地方我会加上注释：
```java
//
// Source code recreated from a .class file by IntelliJ IDEA
// (powered by FernFlower decompiler)
//

package com.test.excel.cglib;

import java.lang.reflect.Method;
import net.sf.cglib.core.ReflectUtils;
import net.sf.cglib.core.Signature;
import net.sf.cglib.proxy.Callback;
import net.sf.cglib.proxy.Factory;
import net.sf.cglib.proxy.MethodInterceptor;
import net.sf.cglib.proxy.MethodProxy;

public class Target$$EnhancerByCGLIB$$e7b1b1b0 extends Target implements Factory {
    private boolean CGLIB$BOUND;
    public static Object CGLIB$FACTORY_DATA;
    private static final ThreadLocal CGLIB$THREAD_CALLBACKS;
    private static final Callback[] CGLIB$STATIC_CALLBACKS;
    // 拦截器
    private MethodInterceptor CGLIB$CALLBACK_0;
    private static Object CGLIB$CALLBACK_FILTER;
    // 被代理方法
    private static final Method CGLIB$execute$0$Method;
    // 代理方法
    private static final MethodProxy CGLIB$execute$0$Proxy;
    private static final Object[] CGLIB$emptyArgs;
    private static final Method CGLIB$equals$1$Method;
    private static final MethodProxy CGLIB$equals$1$Proxy;
    private static final Method CGLIB$toString$2$Method;
    private static final MethodProxy CGLIB$toString$2$Proxy;
    private static final Method CGLIB$hashCode$3$Method;
    private static final MethodProxy CGLIB$hashCode$3$Proxy;
    private static final Method CGLIB$clone$4$Method;
    private static final MethodProxy CGLIB$clone$4$Proxy;

    static void CGLIB$STATICHOOK1() {
        CGLIB$THREAD_CALLBACKS = new ThreadLocal();
        CGLIB$emptyArgs = new Object[0];
        // 代理类
        Class var0 = Class.forName("com.test.excel.cglib.Target$$EnhancerByCGLIB$$e7b1b1b0");
        // 被代理类
        Class var1;
        CGLIB$execute$0$Method = ReflectUtils.findMethods(new String[]{"execute", "()V"}, (var1 = Class.forName("com.test.excel.cglib.Target")).getDeclaredMethods())[0];
        CGLIB$execute$0$Proxy = MethodProxy.create(var1, var0, "()V", "execute", "CGLIB$execute$0");
        Method[] var10000 = ReflectUtils.findMethods(new String[]{"equals", "(Ljava/lang/Object;)Z", "toString", "()Ljava/lang/String;", "hashCode", "()I", "clone", "()Ljava/lang/Object;"}, (var1 = Class.forName("java.lang.Object")).getDeclaredMethods());
        CGLIB$equals$1$Method = var10000[0];
        CGLIB$equals$1$Proxy = MethodProxy.create(var1, var0, "(Ljava/lang/Object;)Z", "equals", "CGLIB$equals$1");
        CGLIB$toString$2$Method = var10000[1];
        CGLIB$toString$2$Proxy = MethodProxy.create(var1, var0, "()Ljava/lang/String;", "toString", "CGLIB$toString$2");
        CGLIB$hashCode$3$Method = var10000[2];
        CGLIB$hashCode$3$Proxy = MethodProxy.create(var1, var0, "()I", "hashCode", "CGLIB$hashCode$3");
        CGLIB$clone$4$Method = var10000[3];
        CGLIB$clone$4$Proxy = MethodProxy.create(var1, var0, "()Ljava/lang/Object;", "clone", "CGLIB$clone$4");
    }

    final void CGLIB$execute$0() {
        super.execute();
    }
    // 被代理的方法

    public final void execute() {
        MethodInterceptor var10000 = this.CGLIB$CALLBACK_0;
        if (var10000 == null) {
            CGLIB$BIND_CALLBACKS(this);
            var10000 = this.CGLIB$CALLBACK_0;
        }

        if (var10000 != null) {
            // 调用拦截器
            var10000.intercept(this, CGLIB$execute$0$Method, CGLIB$emptyArgs, CGLIB$execute$0$Proxy);
        } else {
            super.execute();
        }
    }

    final boolean CGLIB$equals$1(Object var1) {
        return super.equals(var1);
    }

    public final boolean equals(Object var1) {
        MethodInterceptor var10000 = this.CGLIB$CALLBACK_0;
        if (var10000 == null) {
            CGLIB$BIND_CALLBACKS(this);
            var10000 = this.CGLIB$CALLBACK_0;
        }

        if (var10000 != null) {
            Object var2 = var10000.intercept(this, CGLIB$equals$1$Method, new Object[]{var1}, CGLIB$equals$1$Proxy);
            return var2 == null ? false : (Boolean)var2;
        } else {
            return super.equals(var1);
        }
    }

    final String CGLIB$toString$2() {
        return super.toString();
    }

    public final String toString() {
        MethodInterceptor var10000 = this.CGLIB$CALLBACK_0;
        if (var10000 == null) {
            CGLIB$BIND_CALLBACKS(this);
            var10000 = this.CGLIB$CALLBACK_0;
        }

        return var10000 != null ? (String)var10000.intercept(this, CGLIB$toString$2$Method, CGLIB$emptyArgs, CGLIB$toString$2$Proxy) : super.toString();
    }

    final int CGLIB$hashCode$3() {
        return super.hashCode();
    }

    public final int hashCode() {
        MethodInterceptor var10000 = this.CGLIB$CALLBACK_0;
        if (var10000 == null) {
            CGLIB$BIND_CALLBACKS(this);
            var10000 = this.CGLIB$CALLBACK_0;
        }

        if (var10000 != null) {
            Object var1 = var10000.intercept(this, CGLIB$hashCode$3$Method, CGLIB$emptyArgs, CGLIB$hashCode$3$Proxy);
            return var1 == null ? 0 : ((Number)var1).intValue();
        } else {
            return super.hashCode();
        }
    }

    final Object CGLIB$clone$4() throws CloneNotSupportedException {
        return super.clone();
    }

    protected final Object clone() throws CloneNotSupportedException {
        MethodInterceptor var10000 = this.CGLIB$CALLBACK_0;
        if (var10000 == null) {
            CGLIB$BIND_CALLBACKS(this);
            var10000 = this.CGLIB$CALLBACK_0;
        }

        return var10000 != null ? var10000.intercept(this, CGLIB$clone$4$Method, CGLIB$emptyArgs, CGLIB$clone$4$Proxy) : super.clone();
    }

    public static MethodProxy CGLIB$findMethodProxy(Signature var0) {
        String var10000 = var0.toString();
        switch(var10000.hashCode()) {
        case -508378822:
            if (var10000.equals("clone()Ljava/lang/Object;")) {
                return CGLIB$clone$4$Proxy;
            }
            break;
        case 539325408:
            if (var10000.equals("execute()V")) {
                return CGLIB$execute$0$Proxy;
            }
            break;
        case 1826985398:
            if (var10000.equals("equals(Ljava/lang/Object;)Z")) {
                return CGLIB$equals$1$Proxy;
            }
            break;
        case 1913648695:
            if (var10000.equals("toString()Ljava/lang/String;")) {
                return CGLIB$toString$2$Proxy;
            }
            break;
        case 1984935277:
            if (var10000.equals("hashCode()I")) {
                return CGLIB$hashCode$3$Proxy;
            }
        }

        return null;
    }

    public Target$$EnhancerByCGLIB$$e7b1b1b0() {
        CGLIB$BIND_CALLBACKS(this);
    }

    public static void CGLIB$SET_THREAD_CALLBACKS(Callback[] var0) {
        CGLIB$THREAD_CALLBACKS.set(var0);
    }

    public static void CGLIB$SET_STATIC_CALLBACKS(Callback[] var0) {
        CGLIB$STATIC_CALLBACKS = var0;
    }

    private static final void CGLIB$BIND_CALLBACKS(Object var0) {
        Target$$EnhancerByCGLIB$$e7b1b1b0 var1 = (Target$$EnhancerByCGLIB$$e7b1b1b0)var0;
        if (!var1.CGLIB$BOUND) {
            var1.CGLIB$BOUND = true;
            Object var10000 = CGLIB$THREAD_CALLBACKS.get();
            if (var10000 == null) {
                var10000 = CGLIB$STATIC_CALLBACKS;
                if (var10000 == null) {
                    return;
                }
            }

            var1.CGLIB$CALLBACK_0 = (MethodInterceptor)((Callback[])var10000)[0];
        }

    }

    public Object newInstance(Callback[] var1) {
        CGLIB$SET_THREAD_CALLBACKS(var1);
        Target$$EnhancerByCGLIB$$e7b1b1b0 var10000 = new Target$$EnhancerByCGLIB$$e7b1b1b0();
        CGLIB$SET_THREAD_CALLBACKS((Callback[])null);
        return var10000;
    }

    public Object newInstance(Callback var1) {
        CGLIB$SET_THREAD_CALLBACKS(new Callback[]{var1});
        Target$$EnhancerByCGLIB$$e7b1b1b0 var10000 = new Target$$EnhancerByCGLIB$$e7b1b1b0();
        CGLIB$SET_THREAD_CALLBACKS((Callback[])null);
        return var10000;
    }

    public Object newInstance(Class[] var1, Object[] var2, Callback[] var3) {
        CGLIB$SET_THREAD_CALLBACKS(var3);
        Target$$EnhancerByCGLIB$$e7b1b1b0 var10000 = new Target$$EnhancerByCGLIB$$e7b1b1b0;
        switch(var1.length) {
        case 0:
            var10000.<init>();
            CGLIB$SET_THREAD_CALLBACKS((Callback[])null);
            return var10000;
        default:
            throw new IllegalArgumentException("Constructor not found");
        }
    }

    public Callback getCallback(int var1) {
        CGLIB$BIND_CALLBACKS(this);
        MethodInterceptor var10000;
        switch(var1) {
        case 0:
            var10000 = this.CGLIB$CALLBACK_0;
            break;
        default:
            var10000 = null;
        }

        return var10000;
    }

    public void setCallback(int var1, Callback var2) {
        switch(var1) {
        case 0:
            this.CGLIB$CALLBACK_0 = (MethodInterceptor)var2;
        default:
        }
    }

    public Callback[] getCallbacks() {
        CGLIB$BIND_CALLBACKS(this);
        return new Callback[]{this.CGLIB$CALLBACK_0};
    }

    public void setCallbacks(Callback[] var1) {
        this.CGLIB$CALLBACK_0 = (MethodInterceptor)var1[0];
    }

    static {
        CGLIB$STATICHOOK1();
    }
}
```

从代理对象反编译源码可以知道，代理对象继承于Target，拦截器调用intercept()方法，intercept()方法由自定义CGLibProxy实现，所以，最后调用CGLibProxy中的intercept()方法，从而完成了由代理对象访问到目标对象的动态代理实现。

#### **1.2.3、jdk代理和cglib代理的总结**

**JDK动态代理：**

**.** 需要目标类实现接口

**.** 生成的代理类是与目标类平级，实现了共同的接口

**.** 使用反射的方式进行最终方法的调用，性能较低

**CGLIB动态代理：**

**.** 不要求目标类实现接口

**.** 生成的代理类是目标类的子类

**.** final方法不会出现在代理类中

**.** 使用**空间换时间**的思想对最终的方法调用进行了优化，提升了运行时性能。

看到这里，大佬们此时是不是心里会产生个疑问：你的标题不是asm与cglib吗，前面说了这么多cglib代理和jdk代理，和asm有关系吗？哈哈，其实虽然我们没有再前面的内容中介绍asm的相关知识，但是其实字里行间都透漏着asm，整体总结一句就是CGLIB包的底层是通过使用一个小而快的字节码处理框架ASM，来转换字节码并生成新的类。

**二、深入ASM**
-----------

### **2.1、简介**

ASM 是一个 Java [字节码](https://link.juejin.cn?target=https%3A%2F%2Fso.csdn.net%2Fso%2Fsearch%3Fq%3D%25E5%25AD%2597%25E8%258A%2582%25E7%25A0%2581%26spm%3D1001.2101.3001.7020 "https://so.csdn.net/so/search?q=%E5%AD%97%E8%8A%82%E7%A0%81&spm=1001.2101.3001.7020")操控框架。它能被用来动态生成类或者增强既有类的功能。ASM 可以直接产生二进制 class 文件，也可以在类被加载入 Java 虚拟机之前动态改变类行为。Java class 被存储在严格格式定义的 .class 文件里，这些类文件拥有足够的元数据来解析类中的所有元素：类名称、方法、属性以及 Java 字节码（指令）。ASM 从类文件中读入信息后，能够改变类行为，分析类信息，甚至能够根据用户要求生成新类。

与 BCEL 和 SERL 不同，ASM 提供了更为现代的编程模型。对于 ASM 来说，Java class 被描述为一棵树；使用 “Visitor” 模式遍历整个二进制结构；事件驱动的处理方式使得用户只需要关注于对其编程有意义的部分，而不必了解 Java 类文件格式的所有细节：ASM 框架提供了默认的"response taker"处理这一切。（官方文档：_**[asm.ow2.io/asm4-guide.…](https://link.juejin.cn?target=https%3A%2F%2Fasm.ow2.io%2Fasm4-guide.pdf "https://asm.ow2.io/asm4-guide.pdf")**_ \***，** \*_中文文档：_ [cf.jd.com/pages/viewp…](https://link.juejin.cn?target=https%3A%2F%2Fcf.jd.com%2Fpages%2Fviewpage.action%3FpageId%3D1139356247 "https://cf.jd.com/pages/viewpage.action?pageId=1139356247") _）_

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/72b256863af946df86edcd55b6949c78~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp)

流程图

### **2.2、核心api介绍**

ASM框架中的核心类有以下几个：

① ClassReader:该类用来解析编译过的class字节码文件。

② ClassWriter:该类用来重新构建编译后的类，比如说修改类名、属性以及方法，甚至可以生成新的类的字节码文件。

③ ClassAdapter:该类也实现了ClassVisitor接口，它将对它的方法调用委托给另一个ClassVisitor对象。

下面会列举每个核心类的几个核心api供大家参考，后面如果想了解更多的内容，可以直接查看asm的官方文档。

#### **2.2.1、ClassReader**

##### **构造方法**

作用 **：** **获取Class文件，输入源很多种，包括字节流，IO流或者直接加载Class。**

示例 **：** **public ClassReader(final InputStream inputStream)**

##### **accept方法**

作用：**解析**[**字节码**](https://link.juejin.cn?target=https%3A%2F%2Fso.csdn.net%2Fso%2Fsearch%3Fq%3D%25E5%25AD%2597%25E8%258A%2582%25E7%25A0%2581%26spm%3D1001.2101.3001.7020 "https://so.csdn.net/so/search?q=%E5%AD%97%E8%8A%82%E7%A0%81&spm=1001.2101.3001.7020")**中常量池之后的所有元素**

示例：**public void accept(final ClassVisitor classVisitor, final int parsingOptions)**

重要参数解析：**parsingOptions（用于跳过读取字节码时的一些信息选项，有以下四种选择可以选择)**

| **SKIP\_CODE**     | 表示跳过代码扫描，如果你只需要只是类的结构，就可以使用这个。                                                                                                                                                                           |
| ------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **SKIP\_DEBUG**    | 跳过调试信息，ClassReader不会去访问调试信息。 如果设置了这个标志，这些属性既不会被解析也不会被访问(例如ClassVisitor.visitSource,MethodVisitor.visitLocalVariable, MethodVisitor.visitLineNumber MethodVisitor.visitParameter)。 这个会比较常用，当你不需要上面这些方法时候。 |
| **SKIP\_FRAMES**   | 跳过堆栈映射帧.如果设置了这个标志，这些属性既不会被解析也不会被访问（例如：MethodVisitor.visitFrame）.这个标志当ClassWriter.COMPUTE\_FRAME选项被使用时，它会避免访问将被忽略并从头重新计算的帧。                                                                               |
| **EXPAND\_FRAMES** | 用来展开堆栈映射帧的标志，会降低性能。                                                                                                                                                                                      |

#### **2.2.2、ClassWriter**

##### **构造方法**

作用：**用来定义类的属性**

示例：**public ClassWriter(final int flags)**

重要参数解析：

| flag == 0                               | 自己计算栈帧和局部变量以及操作数堆栈的大小 ，也就是你要自己调用visitmax和visitFrame方法。 |
| --------------------------------------- | ------------------------------------------------------ |
| flag == **ClassWriter. COMPUTE\_MAXS**  | 局部变量和操作数堆栈部分的大小会为你计算，还需要调用visitFrame方法设置栈帧。            |
| flag == **ClassWriter.COMPUTE\_FRAMES** | 所有的内容都是自动计算的。 你不必调用visitFrame和visitmax                 |

##### **visit方法**

作用：**用来定义类的属性**

示例 **：** public final void visit(int version, int access, String name, String signature, String superName, String\[\] interfaces)

重要参数解析：

| version    | Java版本号，例如 **V1\_8** 代表**Java 8**                                              |                |
| ---------- | ------------------------------------------------------------------------------ | -------------- |
| access     | Class访问权限，一般默认都是 \*\*ACC\_PUBLIC                                               | ACC\_SUPER\*\* |
| name       | Class文件名，例如：**asm/User**，包名加类名                                                 |                |
| signature  | 类的签名，除非你是泛型类或者实现泛型接口，一般默认null。                                                 |                |
| superName  | 继承的类，很明显所有类默认继承Object。例如：**java/lang/Object ，如果是继承自己写的类Animal，那就是 asm/Animal** |                |
| interfaces | 实现的接口，例如实现自己写的接口**IPrint，** 那就是**new String\[\]{"asm/IPrint"}**                |                |

##### **visitMethod方法**

作用：**用来定义类的方法**

示例 **：** **public final MethodVisitor visitMethod(int access, String name, String descriptor, String signature, String\[\] exceptions)**

重要参数解析：

| access     | 方法的访问权限，也就是public，private等                                                                                                                                                                                                                                                                  |
| ---------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| name       | 方法名: 在Class中，有两个特殊方法名。和，其他的方法都是正常对应Java方法名。 代表的是类的实例构造方法初始化，也就是new 一个类时，肯定会调用的方法。 代表的类的初始化方法，不同于，它不是显示调用的。因为Java虚拟机会自动调用，并且保证在子类的前调用父类的。也就是说，Java虚拟机中，第一个被执行方法的肯定是java.lang.Object。 注意：和执行的时机不一样，的时机早于，是在类被加载阶段的初始化过程中调用方法，而方法的调用是在new一个类的实例的时候。                                            |
| descriptor | 方法的描述符，就是[字节码](https://link.juejin.cn?target=https%3A%2F%2Fso.csdn.net%2Fso%2Fsearch%3Fq%3D%25E5%25AD%2597%25E8%258A%2582%25E7%25A0%2581%26spm%3D1001.2101.3001.7020 "https://so.csdn.net/so/search?q=%E5%AD%97%E8%8A%82%E7%A0%81&spm=1001.2101.3001.7020")对代码中方法的形参和返回值的一个描述。其实就是一个一一对应的模板 |
| signature  | 方法签名，除非方法的参数、返回类型和异常使用了泛型，否则一般为 null。                                                                                                                                                                                                                                                       |
| exceptions | 方法上的异常                                                                                                                                                                                                                                                                                      |

##### **visitField方法**

作用：**用来定义一个变量**

示例：**public final FieldVisitor visitField(int access, String name, String descriptor, String signature, Object value)**

重要参数解析：

| access     | 变量的访问权限，，也就是public，private等                                                                       |
| ---------- | ------------------------------------------------------------------------------------------------- |
| name       | 变量名                                                                                               |
| descriptor | 变量的描述符                                                                                            |
| signature  | 变量的签名，如果没有使用泛型则为null                                                                              |
| value      | 变量的初始值。这个字段仅作用于被final修饰的字段，或者接口中声明的变量。其他默认为null，变量的赋值是通过**MethodVisitor** 的 **visitFieldInsn方法。** |

##### **visitEnd方法**

作用：**用来通知Class已经使用完**

示例：**public final void visitEnd()**

##### **toByteArray方法**

作用：**返回一个字节数组**

示例：**public byte\[\] toByteArray()**

#### **2.2.3、ClassVisiter**

看名字就能看出来，这是一个对Class文件进行观察（扫描）的工具类。因为它是一个抽象类，所以我们只能对其进行继承重写。并且ClassVisitor所有的调用都是由ClassReader来进行回调，就是我们前面accept方法。而这个方法里，执行了对ClassVisitor源码扫描的回调。如下图：

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/06c1390ca681466487d6a3125d0a9358~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp)

##### **构造方法**

示例：**protected ClassVisitor(final int api, final ClassVisitor classVisitor)**

参数解释：

api：**代表是ASM API的版本 ，默认填最新（ASM9）即可。编写代码和代码读取的版本号最好保持一致，不然可能会有一些兼容性的错误。**

**由于\*\*\*\*ClassVisitor所有的调用都是由ClassReader来进行回调，所以其他api咱们不做过多介绍。**

### **2.3、ASM实践**

上面给大家介绍了asm的api，还有一些其他的api例如**MethodVisitor、FieldVisitor等**就不过多介绍了，还是那句话，想深入了解，就看一下官方文档。但是光说不练假把式，接下来我们就对asm做一番实践，由此来体会他的功能之强大。

#### 2.3.1、引入jar
```xml
<dependency>    
    <groupId>org.ow2.asm</groupId>    
    <artifactId>asm</artifactId>    
    <version>9.4</version>
</dependency>
```
#### **2.3.2、生成类**

我们自定义生成一个People的类

```java
package com.test.excel.asm;

import org.objectweb.asm.ClassWriter;
import org.objectweb.asm.FieldVisitor;
import org.objectweb.asm.MethodVisitor;

import java.io.FileOutputStream;

import static jdk.internal.org.objectweb.asm.ClassWriter.COMPUTE_FRAMES;
import static jdk.internal.org.objectweb.asm.ClassWriter.COMPUTE_MAXS;
import static jdk.internal.org.objectweb.asm.Opcodes.*;

/**
 * 文件描述
 *
 * @author dzl
 * @date 2022-11-27 18:49
 */
public class AsmTest {

    public static void main(String[] args) throws Exception {
        testCreateAClass();
    }
    public static void testCreateAClass()throws Exception{
        //新建一个类生成器，COMPUTE_FRAMES，COMPUTE_MAXS这2个参数能够让asm自动更新操作数栈
        ClassWriter cw = new ClassWriter(COMPUTE_FRAMES|COMPUTE_MAXS);
        //生成一个public的类，类路径是com.study.Human
        cw.visit(V1_8, ACC_PUBLIC,"com/test/excel/asm/People",null,"java/lang/Object",null);
        //生成默认的构造方法： public People()
        MethodVisitor mv = cw.visitMethod(ACC_PUBLIC,"<init>","()V",null,null);
        mv.visitVarInsn(ALOAD,0);
        mv.visitMethodInsn(INVOKESPECIAL,"java/lang/Object","<init>","()V",false);
        mv.visitInsn(RETURN);
        mv.visitMaxs(0,0);//更新操作数栈
        mv.visitEnd();//一定要有visitEnd

        //生成成员变量
        //1.生成String类型的成员变量:private String name;
        FieldVisitor fv= cw.visitField(ACC_PRIVATE,"name","Ljava/lang/String;",null,null);
        fv.visitEnd();//不要忘记end
        //2.生成Long类型成员：private long age
        fv=cw.visitField(ACC_PRIVATE,"age","J",null,null);
        fv.visitEnd();

        //3.生成Int类型成员:protected int no
        fv=cw.visitField(ACC_PROTECTED,"no","I",null,null);
        fv.visitEnd();

        //4.生成静态成员变量：public static long score
        fv=cw.visitField(ACC_PUBLIC + ACC_STATIC,"score","J",null,null);

        //5.生成常量：public static final String real_name = "Sand哥"
        fv=cw.visitField(ACC_PUBLIC+ACC_STATIC+ACC_FINAL,"real_name","Ljava/lang/String;",null,"Sand哥");
        fv.visitEnd();

        //6.生成成员方法greet
        mv=cw.visitMethod(ACC_PUBLIC,"greet","(Ljava/lang/String;)I",null,null);
        mv.visitCode();
        mv.visitIntInsn(ALOAD,0);
        mv.visitIntInsn(ALOAD,1);

        //6.1 调用静态方法 System.out.println("Hello");
        mv.visitFieldInsn(GETSTATIC,"java/lang/System","out","Ljava/io/PrintStream;");
//        mv.visitLdcInsn("Hello");//加载字符常量
        mv.visitIntInsn(ALOAD,1);//加载形参
        mv.visitMethodInsn(INVOKEVIRTUAL,"java/io/PrintStream","println","(Ljava/lang/String;)V",false);//打印形参
        //获取类的byte数组
        byte[] classByteData=cw.toByteArray();
        //把类数据写入到class文件,这样你就可以把这个类文件打包供其他的人使用
        FileOutputStream out = new FileOutputStream(AsmTest.class.getResource("/com/test/excel/asm/").getPath() + "People.class");
        out.write(classByteData);
        out.close();
    }
}
```
运行结果：

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/460e90ee0f104ec086f521b826e34a70~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp)

#### **2.3.3、修改已存在的类**

假如我们现在有如下类，我们用asm做如下几个修改：1.增加了一个phone字段 2.删除testA方法 3.将testC方法改成protected 4.新增一个getPhone方法。


```java
/**
 * 文件描述
 *
 * @author dzl
 * @date 2022-11-27 19:14
 */
public class ModifyFunction {

    private int a;

    public void testA(){
        System.out.println("I am A");
    }

    public void testB(){
        System.err.println("===>I am B");
    }

    public int testC(){
        return a;
    }
}
```


利用asm修改该类


```java
package com.test.excel.asm;

import org.objectweb.asm.*;

import java.io.FileOutputStream;

import static jdk.internal.org.objectweb.asm.Opcodes.*;
import static org.objectweb.asm.Opcodes.ASM9;

/**
 * 文件描述
 *
 * @author dzl
 * @date 2022-11-27 18:49
 */
public class AsmTest {

    public static void main(String[] args) throws Exception {
        testModifyCalss();
    }
    private static void testModifyCalss()throws Exception{
        ClassReader cr = new ClassReader("com.test.excel.asm.ModifyFunction");
        final ClassWriter cw=new ClassWriter(cr,0);
//        cr.accept(cw, 0);//可以直接接受一个writer,实现复制
        cr.accept(new ClassVisitor(ASM9,cw) {//接受一个带classWriter的visitor，实现定制化方法拷贝或者属性删除字段
            @Override
            public MethodVisitor visitMethod(int access, String name, String descriptor, String signature, String[] exceptions) {
                System.out.println("visit method:"+name+"====> "+descriptor);

                if("testA".equals(name)){//拷贝的过程中删除一个方法
                    return null;
                }

                if("testC".equals(name)){//将testC public方法变成protect
                    access=ACC_PROTECTED;
                }
                return super.visitMethod(access, name, descriptor, signature, exceptions);
            }

            @Override
            public void visitEnd() {
                //特别注意的是：要为类增加属性和方法，放到visitEnd中，避免破坏之前已经排列好的类结构，在结尾添加新结构
                //增加一个字段（注意不能重复）,注意最后都要visitEnd
                FieldVisitor fv = cv.visitField(ACC_PUBLIC, "phone", "Ljava/lang/String;", null, null);
                fv.visitEnd();//不能缺少visitEnd
                //增加一个方法
                MethodVisitor mv=cv.visitMethod(ACC_PUBLIC,"getPhone","()Ljava/lang/String;",null,null);
                mv.visitCode();
                mv.visitVarInsn(ALOAD, 0);
                mv.visitFieldInsn(GETFIELD,"com/test/excel/asm/ModifyFunction","phone","Ljava/lang/String;");
                mv.visitInsn(IRETURN);
                mv.visitMaxs(1, 1);
                mv.visitEnd();//不能缺少visitEnd
                super.visitEnd();//注意原本的visiEnd不能少
            }
        },0);
        //指定新生成的class路径的生成位置,这个路径你可以随便指定
        FileOutputStream out = new FileOutputStream(AsmTest.class.getResource("/com/test/excel/asm/").getPath() + "ModifyFunction.class");
        out.write(cw.toByteArray());
        out.close();
    }
}

```
运行结果：

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/57cbc292b1294cb5b0e10969c727918f~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp)
