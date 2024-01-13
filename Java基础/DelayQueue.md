
# offer


```java
public E poll() {
	final ReentrantLock lock = this.lock;
	lock.lock();
	try {
		E first = q.peek();
		// 队列为空或者首节点剩余延迟时间大于0
		return (first == null || first.getDelay(NANOSECONDS) > 0)
			? null
			: q.poll();
	} finally {
		lock.unlock();
	}
}

```