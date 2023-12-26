
先对线程池里面的一些属性介绍一下：
```java
private static final int COUNT_BITS = Integer.SIZE - 3;
private static final int COUNT_MASK = (1 << COUNT_BITS) - 1;
private static int runStateOf(int c)     { return c & ~COUNT_MASK; }
private static int workerCountOf(int c)  { return c & COUNT_MASK; }
```

上面用到了`Integer.SIZE`值是32
```java
@Native public static final int SIZE = 32;
```


看一下任务是如何提交的
```java
public Future<?> submit(Runnable task) {
	if (task == null) throw new NullPointerException();
	RunnableFuture<Void> ftask = newTaskFor(task, null);
	execute(ftask);
	return ftask;
}

```

```java
public void execute(Runnable command) {
	if (command == null)
		throw new NullPointerException();
	/*
	 * Proceed in 3 steps:
	 *
	 * 1. If fewer than corePoolSize threads are running, try to
	 * start a new thread with the given command as its first
	 * task.  The call to addWorker atomically checks runState and
	 * workerCount, and so prevents false alarms that would add
	 * threads when it shouldn't, by returning false.
	 *
	 * 2. If a task can be successfully queued, then we still need
	 * to double-check whether we should have added a thread
	 * (because existing ones died since last checking) or that
	 * the pool shut down since entry into this method. So we
	 * recheck state and if necessary roll back the enqueuing if
	 * stopped, or start a new thread if there are none.
	 *
	 * 3. If we cannot queue task, then we try to add a new
	 * thread.  If it fails, we know we are shut down or saturated
	 * and so reject the task.
	 */
	int c = ctl.get();
	if (workerCountOf(c) < corePoolSize) {
		if (addWorker(command, true))
			return;
		c = ctl.get();
	}
	if (isRunning(c) && workQueue.offer(command)) {
		int recheck = ctl.get();
		if (! isRunning(recheck) && remove(command))
			reject(command);
		else if (workerCountOf(recheck) == 0)
			addWorker(null, false);
	}
	else if (!addWorker(command, false))
		reject(command);
}

```

> [!NOTE] 核心线程数为空的时候，线程池如何处理任务
> Contents
