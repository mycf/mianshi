NIO 全称 Non-Blocking I/O，也有人称之为 New I/O，它是 JDK 1.4 中引入的一种同步非阻塞的 I/O 模型。它弥补了传统 I/O 的不足，是解决大量连接与高并发的有效方式。Java NIO 适用于连接数较多且连接时间较短的场景。
NIO 是基于 Reactor 反应堆模式实现的，简单来说，反应堆模式包含三步：
1. 注册感兴趣的事件;
2. 扫描是否有感兴趣的事件发生;
3. 事件发生后做出相应的处理:

NIO 包含三大核心组件：Channel 通道、Buffer 缓冲区 和 Selector 选择器

Channel 是应用程序和操作系统交互事件、传递数据的通道，通道中的数据总是要先读到一个 Buffer，或者从一个 Buffer 中写入。所有被 Selector 注册的通道，都只能是继承了 SelectableChannel 类的子类。Java NIO 中的 Channel 主要有以下几种类型：
- FileChannel：用于本地文件的读取、写入、映射和操作的通道
- DatagramChannel：可用于实现发送和接收 UDP 协议数据包的通道
- ServerSocketChannel：应用服务程序的监听通道，可视为被 Channel 包装的 ServerSocket;
- SocketChannel：TCP Socket 的监听通道，可视为被 Channel 包装的 Socket;

Buffer 缓冲区，主要用于和 Channel 进行交互。以写为例，应用程序先将数据写入缓冲区，再通过通道把缓冲区的数据发送出去;读也是一样，数据先从通道读到缓冲区，应用程序再读取缓冲区的数据。Buffer 缓冲区本质上是一个字节数组，Buffer 读写数据一般遵循如下步骤:
1.写数据到 Buffer 缓冲区;
2.调用 flip()方法将 Buffer 从写模式切换到读模式;
3.从 Buffer 中读取数据;
4.调用 clear()方法或者 compact()方法清理数据，准备下一次的写入;
常见的 Buffer 实现有:ByteBuffer, CharBuffer, DoubleBuffer, FloatBuffer, IntBuffer, LongBuffer.
ShortBuffer;此外还有 MappedByteBuffer, HeapByteBuffer, DirectByteBuffer 等。