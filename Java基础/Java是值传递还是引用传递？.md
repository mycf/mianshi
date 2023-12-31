# Java是值传递还是引用传递？

(来自知乎)[https://www.zhihu.com/question/31203609/answer/576030121]

> 错误理解一：值传递和引用传递，区分的条件是传递的内容，如果是个值，就是值传递。如果是个引用，就是引用传递。
> 错误理解二：Java是引用传递。
> 错误理解三：传递的参数如果是普通类型，那就是值传递，如果是对象，那就是引用传递。

## 实参与形参

我们都知道，在Java中定义方法的时候是可以定义参数的。比如Java中的main方法，public static void main(String[] args)，这里面的args就是参数。参数在程序语言中分为形式参数和实际参数。

- 形式参数：是在定义函数名和函数体的时候使用的参数,目的是用来接收调用该函数时传入的参数。
- 实际参数：在调用有参函数时，主调函数和被调函数之间有数据传递关系。在主调函数中调用一个函数时，函数名后面括号中的参数称为“实际参数”。

简单举个例子：

```java
public static void main(String[] args) {
    ParamTest pt = new ParamTest();
    pt.sout("Hollis");//实际参数为 Hollis
}

public void sout(String name) { //形式参数为 name
    System.out.println(name);
}
```

实际参数是调用有参方法的时候真正传递的内容，而形式参数是用于接收实参内容的参数。

## 值传递与引用传递

上面提到了，当我们调用一个有参函数的时候，会把实际参数传递给形式参数。但是，在程序语言中，这个传递过程中传递的两种情况，即值传递和引用传递。我们来看下程序语言中是如何定义和区分值传递和引用传递的。

- 值传递（pass by value）是指在调用函数时将实际参数复制一份传递到函数中，这样在函数中如果对参数进行修改，将不会影响到实际参数。
- 引用传递（pass by reference）是指在调用函数时将实际参数的地址直接传递到函数中，那么在函数中对参数所进行的修改，将影响到实际参数。

有了上面的概念，然后大家就可以写代码实践了，来看看Java中到底是值传递还是引用传递 ，于是，最简单的一段代码出来了：

```java
public static void main(String[] args) {
    ParamTest pt = new ParamTest();

    int i = 10;
    pt.pass(10);
    System.out.println("print in main , i is " + i);
}

public void pass(int j) {
    j = 20;
    System.out.println("print in pass , j is " + j);
}
```

上面的代码中，我们在pass方法中修改了参数j的值，然后分别在pass方法和main方法中打印参数的值。输出结果如下：

```sh
print in pass , j is 20
print in main , i is 10
```

可见，pass方法内部对name的值的修改并没有改变实际参数i的值。那么，按照上面的定义，有人得到结论：Java的方法传递是值传递。

但是，很快就有人提出质疑了（哈哈，所以，不要轻易下结论咯。）。然后，他们会搬出以下代码：

```java
public static void main(String[] args) {
    ParamTest pt = new ParamTest();

    User hollis = new User();
    hollis.setName("Hollis");
    hollis.setGender("Male");
    pt.pass(hollis);
    System.out.println("print in main , user is " + hollis);
}

public void pass(User user) {
    user.setName("hollischuang");
    System.out.println("print in pass , user is " + user);
}
```
同样是一个pass方法，同样是在pass方法内修改参数的值。输出结果如下：

```
print in pass , user is User{name='hollischuang', gender='Male'}
print in main , user is User{name='hollischuang', gender='Male'}
```

经过pass方法执行后，实参的值竟然被改变了，那按照上面的引用传递的定义，实际参数的值被改变了，这不就是引用传递了么。于是，根据上面的两段代码，有人得出一个新的结论：Java的方法中，在传递普通类型的时候是值传递，在传递对象类型的时候是引用传递。

但是，这种表述仍然是错误的。不信你看下面这个参数类型为对象的参数传递：

```java
public static void main(String[] args) {
    ParamTest pt = new ParamTest();

    String name = "Hollis";
    pt.pass(name);
    System.out.println("print in main , name is " + name);
}

public void pass(String name) {
    name = "hollischuang";
    System.out.println("print in pass , name is " + name);
}
```
上面的代码输出结果为
```sh
print in pass , name is hollischuang
print in main , name is Hollis
```

这又作何解释呢？同样传递了一个对象，但是原始参数的值并没有被修改，难道传递对象又变成值传递了？

Java中的值传递
上面，我们举了三个例子，表现的结果却不一样，这也是导致很多初学者，甚至很多高级程序员对于Java的传递类型有困惑的原因。

其实，我想告诉大家的是，上面的概念没有错，只是代码的例子有问题。来，我再来给大家画一下概念中的重点，然后再举几个真正恰当的例子。

> 值传递（pass by value）是指在调用函数时将实际参数复制一份传递到函数中，这样在函数中如果对参数进行修改，将不会影响到实际参数。
> 引用传递（pass by reference）是指在调用函数时将实际参数的地址直接传递到函数中，那么在函数中对参数所进行的修改，将影响到实际参数。

那么，我来给大家总结一下，值传递和引用传递之前的区别的重点是什么。

我们上面看过的几个pass的例子中，都只关注了实际参数内容是否有改变。如传递的是User对象，我们试着改变他的name属性的值，然后检查是否有改变。其实，在实验方法上就错了，当然得到的结论也就有问题了。

为什么说实验方法错了呢？这里我们来举一个形象的例子。再来深入理解一下值传递和引用传递，然后你就知道为啥错了。

你有一把钥匙，当你的朋友想要去你家的时候，如果你直接把你的钥匙给他了，这就是引用传递。这种情况下，如果他对这把钥匙做了什么事情，比如他在钥匙上刻下了自己名字，那么这把钥匙还给你的时候，你自己的钥匙上也会多出他刻的名字。

你有一把钥匙，当你的朋友想要去你家的时候，你复刻了一把新钥匙给他，自己的还在自己手里，这就是值传递。这种情况下，他对这把钥匙做什么都不会影响你手里的这把钥匙。

但是，不管上面那种情况，你的朋友拿着你给他的钥匙，进到你的家里，把你家的电视砸了。那你说你会不会受到影响？而我们在pass方法中，改变user对象的name属性的值的时候，不就是在“砸电视”么。

还拿上面的一个例子来举例，我们真正的改变参数，看看会发生什么？

```java
public static void main(String[] args) {
    ParamTest pt = new ParamTest();

    User hollis = new User();
    hollis.setName("Hollis");
    hollis.setGender("Male");
    pt.pass(hollis);
    System.out.println("print in main , user is " + hollis);
}

public void pass(User user) {
    user = new User();
    user.setName("hollischuang");
    user.setGender("Male");
    System.out.println("print in pass , user is " + user);
}
```


上面的代码中，我们在pass方法中，改变了user对象，输出结果如下：
```sh
print in pass , user is User{name='hollischuang', gender='Male'}
print in main , user is User{name='Hollis', gender='Male'}
```

我们来画一张图，看一下整个过程中发生了什么，然后我再告诉你，为啥Java中只有值传递。


