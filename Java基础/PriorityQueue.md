**优先队列的作用是能保证每次取出的元素都是队列中权值最小的**(Java的优先队列每次取最小元素，C++的优先队列每次取最大元素)。

Java中*PriorityQueue*实现了*Queue*接口，不允许放入`null`元素；其通过堆实现，具体说是通过完全二叉树(_complete binary tree_)实现的**小顶堆**(任意一个非叶子节点的权值，都不大于其左右子节点的权值)，也就意味着可以通过数组来作为*PriorityQueue*的底层实现。

![[PriorityQueue 2024-01-12 21.38.39.excalidraw|100%]]

```java
// 默认容量
private static final int DEFAULT_INITIAL_CAPACITY = 11;
// 实际存储元素的数组
transient Object[] queue; // non-private to simplify nested class access

// 元素数量
int size;

@SuppressWarnings("serial") // Conditionally serializable
// 比较器
private final Comparator<? super E> comparator;

// 修改次数
transient int modCount;     // non-private to simplify nested class access
```

# 构造函数

```java
public PriorityQueue() {
	this(DEFAULT_INITIAL_CAPACITY, null);
}

public PriorityQueue(int initialCapacity) {
	this(initialCapacity, null);
}

```

# offer

```java
public boolean offer(E e) {
	if (e == null) // 不允许放入null
		throw new NullPointerException();
	modCount++;
	int i = size;
	// 实际元素数量大于等于队列大小
	if (i >= queue.length)
		// 自动扩容
		grow(i + 1);
	// 
	siftUp(i, e); //调整
	size = i + 1;
	return true;
}

```

![[PriorityQueue 2024-01-12 22.21.34.excalidraw|100%]]

```java
// k 索引位置
// x 元素
private void siftUp(int k, E x) {
	if (comparator != null)
		// comparator是内部变量，把它传过去有点无聊
		siftUpUsingComparator(k, x, queue, comparator);
	else
		siftUpComparable(k, x, queue);
}

private static <T> void siftUpComparable(int k, T x, Object[] es) {
	Comparable<? super T> key = (Comparable<? super T>) x;
	while (k > 0) {
		int parent = (k - 1) >>> 1; // parentNo = (nodeNo-1)/2
		Object e = es[parent];
		if (key.compareTo((T) e) >= 0)
			break;
		// 和父节点进行交换
		es[k] = e;
		k = parent;
	}
	es[k] = key;
}

private static <T> void siftUpUsingComparator(
	int k, T x, Object[] es, Comparator<? super T> cmp) {
	while (k > 0) {
		int parent = (k - 1) >>> 1;
		Object e = es[parent];
		if (cmp.compare(x, (T) e) >= 0)
			break;
		es[k] = e;
		k = parent;
	}
	es[k] = x;
}
```

