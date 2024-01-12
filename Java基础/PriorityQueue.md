
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

