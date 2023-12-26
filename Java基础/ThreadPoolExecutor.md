
```java
public Future<?> submit(Runnable task) {
	if (task == null) throw new NullPointerException();
	RunnableFuture<Void> ftask = newTaskFor(task, null);
	execute(ftask);
	return ftask;
}

```

> [!NOTE] 核心线程数为空的时候，线程池如何处理任务
> Contents
