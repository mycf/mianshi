# 为什么finalize方法非常不好，非常影响性能

1.finalize方法的调用的次序不能保证

2.Finalizer线程是守护线程，finalize中的代码不能保证执行。

3.执行finalize中的方法出现了异常，不会抛出

4.finalize的方法在垃圾回收时不会调用，而是放到finalize的queue中，


