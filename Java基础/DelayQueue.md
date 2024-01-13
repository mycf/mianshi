
# offer

# poll

![[Queue#`E poll()`：]]
```java
public E poll() {
	final ReentrantLock lock = this.lock;
	lock.lock();
	try {
		E first = q.peek();
		// 队列为空或者首节点剩余延迟时间大于0，返回null
		return (first == null || first.getDelay(NANOSECONDS) > 0)
			? null
			// 否则返回首元素
			: q.poll();
	} finally {
		lock.unlock();
	}
}

```
![[Queue#`E peek()`：]]
# take
![[BlockingQueue#`E take()`：]]
```java
public E take() throws InterruptedException {
	final ReentrantLock lock = this.lock;
	// 可响应中断
	lock.lockInterruptibly();
	try {
		for (;;) {
			E first = q.peek();
			if (first == null)
				available.await();
			else {
				long delay = first.getDelay(NANOSECONDS);
				if (delay <= 0L)
					return q.poll();
				first = null; // don't retain ref while waiting
				if (leader != null)
					available.await();
				else {
					Thread thisThread = Thread.currentThread();
					leader = thisThread;
					try {
						available.awaitNanos(delay);
					} finally {
						if (leader == thisThread)
							leader = null;
					}
				}
			}
		}
	} finally {
		if (leader == null && q.peek() != null)
			available.signal();
		lock.unlock();
	}
}
```