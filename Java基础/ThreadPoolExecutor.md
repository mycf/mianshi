==线程池的处理流程：==
![[ThreadPoolExecutor 2023-12-26 21.48.29.excalidraw]]
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

```java
private final AtomicInteger ctl = new AtomicInteger(ctlOf(RUNNING, 0));
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
	// 当还没有达到核心线程池的数量时，直接添加1个新线程，然后让其执行任务即可
	if (workerCountOf(c) < corePoolSize) {
		// 添加新线程，且执行command任务
		// 添加成功，即不需要后续操作了，添加失败，则说明外部环境变化了
		if (addWorker(command, true))
			return;
		c = ctl.get();
	}
	// 当核心线程达到后，则尝试添加到阻塞队列中，具体添加方法由阻塞队列实现
	if (isRunning(c) && workQueue.offer(command)) {
		int recheck = ctl.get();
		// 添加队列成功后，还要再次检测线程池的运行状态，决定启动线程或者状态过期
		// 当线程池已关闭，则将刚刚添加的任务移除，走reject策略
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


```java
final void runWorker(Worker w) {
	Thread wt = Thread.currentThread();
	Runnable task = w.firstTask;
	w.firstTask = null;
	w.unlock(); // allow interrupts
	boolean completedAbruptly = true;
	try {
		while (task != null || (task = getTask()) != null) {
			w.lock();
			// If pool is stopping, ensure thread is interrupted;
			// if not, ensure thread is not interrupted.  This
			// requires a recheck in second case to deal with
			// shutdownNow race while clearing interrupt
			if ((runStateAtLeast(ctl.get(), STOP) ||
				 (Thread.interrupted() &&
				  runStateAtLeast(ctl.get(), STOP))) &&
				!wt.isInterrupted())
				wt.interrupt();
			try {
				beforeExecute(wt, task);
				try {
					task.run();
					afterExecute(task, null);
				} catch (Throwable ex) {
					afterExecute(task, ex);
					throw ex;
				}
			} finally {
				task = null;
				w.completedTasks++;
				w.unlock();
			}
		}
		completedAbruptly = false;
	} finally {
		processWorkerExit(w, completedAbruptly);
	}
}
```


> [!NOTE]  线程池的线程是怎么复用的
> 正常可能以为线程xl

