
# offer

# poll
![[Queue#`E peek()`：]]
![[Queue#4. `E poll()`：]]

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