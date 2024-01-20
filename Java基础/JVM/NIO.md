NIO 全称 Non-Blocking I/0，也有人称之为 New I/O，它是 JDK 1.4 中引入的一种同步非阻塞的 I/O 模型。它弥补了传统 1/0 的不足，是解决大量连接与高并发的有效方式。Java NIO 适用于连接数较多且连接时间较短的场景。
NIO 是基于 Reactor 反应堆模式实现的，简单来说，反应堆模式包含三步
1.注册感兴趣的事件;
2.扫描是否有感兴趣的事件发生;
3.事件发生后做出相应的处理:
NIO 包含三大核心组件:Channel 通道、Buffer 缓冲区 和 Selector 选择器
Channel 是应用程序和操作系统交互事件、传递数据的通道，通道中的数据总是要先读到一个 Buffer，或者从一个 Buffer 中写入。所有被 Selector 注册的通道，都只能是继承了 SelectableChannel 类的子类。Java N10 中的 Channel 主要有以下几种类型:
·FileChannel:用于本地文件的读取、写入、映射和操作的通道·DatagramChannel:可用于实现发送和接收 UDP 协议数据包的通道