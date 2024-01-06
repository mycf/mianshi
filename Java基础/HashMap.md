
# 1.7

> [!NOTE] 1.7 `HashMap` 为什么采用头插法
> HashMap的链表是单链表，如果插到尾部需要遍历整个链表。

> [!NOTE] HashMap为什么使用2的次方
> ==方便定位下标==，`HashMap`使用了`&`操作定位下标，indexFor =》h & (length - 1)
```java
static int indexFor(int h, int length) {
	// assert Integer.bitCount(length) == 1 : "length must be a non-zero power of 2";
	return h & (length-1);
}

```

## HashMap的扩容
```java
void addEntry(int hash, K key, V value, int bucketIndex) {
	// 到达阈值，并且下标的节点不为空
	if ((size >= threshold) && (null != table[bucketIndex])) {
		// 进行扩容
		resize(2 * table.length);
		hash = (null != key) ? hash(key) : 0;
		bucketIndex = indexFor(hash, table.length);
	}

	createEntry(hash, key, value, bucketIndex);
}
```

```java
void transfer(Entry<?,?>[] newTable, boolean rehash) {
	Entry<?,?>[] src = table;
	int newCapacity = newTable.length;
	for (int j = 0; j < src.length; j++) {
		Entry<K,V> e = (Entry<K,V>)src[j];
		while(null != e) {
			Entry<K,V> next = e.next;
			if (rehash) {
				e.hash = null == e.key ? 0 : hash(e.key);
			}
			int i = indexFor(e.hash, newCapacity);
			e.next = (Entry<K,V>)newTable[i];
			newTable[i] = e;
			e = next;
		}
	}
}

```
因此，HashMap正常情况下的扩容就是是这样一个过程。我们来看，旧HashMap的节点会依次转移到新的HashMap中，旧HashMap转移链表元素的顺序是A、B、C，而新HashMap使用的是头插法插入，所以，扩容完成后最终在新HashMap中链表元素的顺序是C、B、A。
![[头插法.excalidraw]]

```java
final boolean initHashSeedAsNeeded(int capacity) {
	// hashSeed hash种子 hashSeed一开始是0，所以刚开始这里是false
	boolean currentAltHashing = hashSeed != 0;
	boolean useAltHashing = sun.misc.VM.isBooted() &&
			(capacity >= Holder.ALTERNATIVE_HASHING_THRESHOLD);
	// 异或不相同为true
	boolean switching = currentAltHashing ^ useAltHashing;
	if (switching) {
		// 重新生成hash种子 只有这里会改hash种子
		hashSeed = useAltHashing
			? sun.misc.Hashing.randomHashSeed(this)
			: 0;
	}
	return switching;
}

```

`Holder.ALTERNATIVE_HASHING_THRESHOLD`的赋值逻辑如下：正常都是`Integer.MAX_VALUE`
```java
static final int ALTERNATIVE_HASHING_THRESHOLD;

static {
	// 正常是空
	String altThreshold = java.security.AccessController.doPrivileged(
		new sun.security.action.GetPropertyAction(
			// 自己配置启动参数-Djdk.map.althashing.threshold
			// 一般不会自己配置
			"jdk.map.althashing.threshold"));

	int threshold;
	try {
		// 正常都是Integer.MAX_VALUE;
		threshold = (null != altThreshold)
				? Integer.parseInt(altThreshold)
				: ALTERNATIVE_HASHING_THRESHOLD_DEFAULT; // Integer.MAX_VALUE;

		// disable alternative hashing if -1
		if (threshold == -1) {
			threshold = Integer.MAX_VALUE;
		}

		if (threshold < 0) {
			throw new IllegalArgumentException("value must be positive integer.");
		}
	} catch(IllegalArgumentException failed) {
		throw new Error("Illegal value for 'jdk.map.althashing.threshold'", failed);
	}

	// 正常都是Integer.MAX_VALUE;
	ALTERNATIVE_HASHING_THRESHOLD = threshold;
```

# JDK7 死锁问题
![[HashMap死循环.excalidraw|100%]]