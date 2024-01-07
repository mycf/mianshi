
```java
public class UnSafeDemo {
    static final Unsafe UNSAFE;
    int i = 1;
    final static long offset;

    static {
        try {
            Field unsafe = Unsafe.class.getDeclaredField("theUnsafe");
            unsafe.setAccessible(true);
            UNSAFE = (Unsafe) unsafe.get(Unsafe.class);
            offset = UNSAFE.objectFieldOffset(UnSafeDemo.class.getDeclaredField("i"));

        } catch (Exception e) {
            throw new RuntimeException(e);
        }

    }
    public static void main(String[] args) {
        UnSafeDemo unSafeDemo = new UnSafeDemo();
        System.out.println(UNSAFE.getIntVolatile(unSafeDemo, offset));
    }
}
```