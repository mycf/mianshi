
```java
// 实际存储元素的数组
transient Object[] queue; // non-private to simplify nested class access

/**
 * The number of elements in the priority queue.
 */
// 
int size;

/**
 * The comparator, or null if priority queue uses elements'
 * natural ordering.
 */
@SuppressWarnings("serial") // Conditionally serializable
private final Comparator<? super E> comparator;

/**
 * The number of times this priority queue has been
 * <i>structurally modified</i>.  See AbstractList for gory details.
 */
transient int modCount;     // non-private to simplify nested class access

```