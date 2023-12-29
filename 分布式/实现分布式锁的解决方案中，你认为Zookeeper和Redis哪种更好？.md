1. 为什么使用分布式锁
使用分布式锁的一个核心目的是为了解决在同一个时刻，多个进程或者多个线程去访问某个共享资源所带来的安全性问题。
而根据锁的用途，可以分为两类：
第一类是共享锁，就是在同一时刻允许多个进程或者线程去访问某个共享资源，可以适用幂等性的场景中，从而避免重复加锁带来的性能开销。
第二类是排它锁，在同一个时刻只允许一个线程或者进程访问共享资源，适合在非幂等性的场景中。

目前常使用redis和zookeeper实现分布式锁。

Redis可以通过两种方式实现分布式锁：
第一种是基于Redis本身提供的setnx指令，setnx设置key时，如果key已存在会返回0，表示无法设置，否则返回1，表示设置成功。通过机制去判断是否获得锁
第二种是基于redisson，redisson提供了分布式锁的封装方法，只需要调用api就行了。redisson里面的指令都是通过lua脚本实现的，lua脚本可以保证脚本执行的原子性。另外，redisson还提供了watchdog，翻译过来就是看门狗，它会在获取锁之后，每隔10秒钟把key的时间续约，从而避免锁的过期。

zookeeper实现分布式锁：
通过临时有序节点实现，线程去zookeeper

Redis获取锁简单粗暴，如果取不到锁，就会不断尝试，这里会占用cpu资源，对整个应用程序造成很大影响
Redis是AP的模型，也就是可用性模型，在集群模式中，很容易存在数据一致性，导致锁出现问题，即便使用redlock算法来实现分布式锁，但是在某些复杂场景下，也无法保证锁的100%可用。不过在实际开发中使用redis实现分布式锁还是比较常见的，因为大部分的情况下，都不会遇到一些极端复杂的场景；更重要的一个点是Redis的性能是比较高的，所以在高并发场景中，比较合适一点。
Zookeeper天生的设计定位就是一个分布式协调组件，是CP模型，所以它更适合用来实现分布式锁。Zookeeper如果获取不到锁，只需要添加一个监听器就行了，不用一直去轮询，所以性能消耗小一点。
如果在两者之间做选择的话，比较推荐zk实现分布式锁。因为分布式锁来说，是一个CP模型的场景，而Redis是AP模型的场景，所以Zookeeper更加适合一点。



| 步骤 | bb |  |
| ---- | ---- | ---- |
|  |  |  |
|  |  |  |
![[分布式锁 2023-12-26 12.53.41.excalidraw]]



## 14.0 Zookeeper 分布式锁实现原理

	分布式锁是控制分布式系统之间同步访问共享资源的一种方式。

下面介绍 zookeeper 如何实现分布式锁，讲解排他锁和共享锁两类分布式锁。

### 排他锁

排他锁（Exclusive Locks），又被称为写锁或独占锁，如果事务T1对数据对象O1加上排他锁，那么整个加锁期间，只允许事务T1对O1进行读取和更新操作，其他任何事务都不能进行读或写。

定义锁：

/exclusive_lock/lock

**实现方式：**

利用 zookeeper 的同级节点的唯一性特性，在需要获取排他锁时，所有的客户端试图通过调用 create() 接口，在 **/exclusive_lock** 节点下创建临时子节点 **/exclusive_lock/lock**，最终只有一个客户端能创建成功，那么此客户端就获得了分布式锁。同时，所有没有获取到锁的客户端可以在 **/exclusive_lock** 节点上注册一个子节点变更的 watcher 监听事件，以便重新争取获得锁。

### 共享锁

共享锁（Shared Locks），又称读锁。如果事务T1对数据对象O1加上了共享锁，那么当前事务只能对O1进行读取操作，其他事务也只能对这个数据对象加共享锁，直到该数据对象上的所有共享锁都释放。

定义锁:

/shared_lock/[hostname]-请求类型W/R-序号

**实现方式：**

1、客户端调用 create 方法创建类似定义锁方式的临时顺序节点。

![](https://www.runoob.com/wp-content/uploads/2020/09/lock-01.png)

2、客户端调用 getChildren 接口来获取所有已创建的子节点列表。

3、判断是否获得锁，对于读请求如果所有比自己小的子节点都是读请求或者没有比自己序号小的子节点，表明已经成功获取共享锁，同时开始执行度逻辑。对于写请求，如果自己不是序号最小的子节点，那么就进入等待。

4、如果没有获取到共享锁，读请求向比自己序号小的最后一个写请求节点注册 watcher 监听，写请求向比自己序号小的最后一个节点注册watcher 监听。

实际开发过程中，可以 curator 工具包封装的API帮助我们实现分布式锁。

<dependency> <groupId>org.apache.curator</groupId> <artifactId>curator-recipes</artifactId> <version>x.x.x</version> </dependency>

curator 的几种锁方案 ：

- 1、**InterProcessMutex**：分布式可重入排它锁
- 2、**InterProcessSemaphoreMutex**：分布式排它锁
- 3、**InterProcessReadWriteLock**：分布式读写锁

下面例子模拟 50 个线程使用重入排它锁 InterProcessMutex 同时争抢锁：

## 实例

public class InterprocessLock {  
    public static void main(String[] args)  {  
        CuratorFramework zkClient = getZkClient();  
        String lockPath = "/lock";  
        InterProcessMutex lock = new InterProcessMutex(zkClient, lockPath);  
        //模拟50个线程抢锁  
        for (int i = 0; i < 50; i++) {  
            new Thread(new TestThread(i, lock)).start();  
        }  
    }  
  
  
    static class TestThread implements Runnable {  
        private Integer threadFlag;  
        private InterProcessMutex lock;  
  
        public TestThread(Integer threadFlag, InterProcessMutex lock) {  
            this.threadFlag = threadFlag;  
            this.lock = lock;  
        }  
  
        @Override  
        public void run() {  
            try {  
                lock.acquire();  
                System.out.println("第"+threadFlag+"线程获取到了锁");  
                //等到1秒后释放锁  
                Thread.sleep(1000);  
            } catch (Exception e) {  
                e.printStackTrace();  
            }finally {  
                try {  
                    lock.release();  
                } catch (Exception e) {  
                    e.printStackTrace();  
                }  
            }  
        }  
    }  
  
    private static CuratorFramework getZkClient() {  
        String zkServerAddress = "192.168.3.39:2181";  
        ExponentialBackoffRetry retryPolicy = new ExponentialBackoffRetry(1000, 3, 5000);  
        CuratorFramework zkClient = CuratorFrameworkFactory.builder()  
                .connectString(zkServerAddress)  
                .sessionTimeoutMs(5000)  
                .connectionTimeoutMs(5000)  
                .retryPolicy(retryPolicy)  
                .build();  
        zkClient.start();  
        return zkClient;  
    }  
}  

控制台每间隔一秒钟输出一条记录：

![](https://www.runoob.com/wp-content/uploads/2020/09/lock-02.png)

