# Linux网络I/O模型简介

Linux的内核将所有外部设备都看做一个文件来操作，对一个文件的读写操作会调用内核提供的系统命令，返回一个file descriptor（fd，文件描述符）。而对一个socket的读写也会有相应的描述符，称为socketfd（socket描述符），描述符就是一个数字，它指向内核中的一个结构体（文件路径，数据区等一些属性）。

根据UNIX网络编程对I/O模型的分类，UNIX提供了5种I/O模型，分别如下。

（1）阻塞I/O模型：最常用的I/O模型就是阻塞I/O模型，缺省情形下，所有文件操作都是阻塞的。我们以套接字接口为例来讲解此模型：在进程空间中调用recvfrom，其系统调用直到数据包到达且被复制到应用进程的缓冲区中或者发生错误时才返回，在此期间一直会等待，进程在从调用recvfrom开始到它返回的整段时间内都是被阻塞的，因此被称为阻塞I/O模型，如图1-1所示。

（2）非阻塞I/O模型：recvfrom从应用层到内核的时候，如果该缓冲区没有数据的话，就直接返回一个EWOULDBLOCK错误，一般都对非阻塞I/O模型进行轮询检查这个状态，看内核是不是有数据到来，如图1-2所示。

（3）I/O复用模型：Linux提供select/poll，进程通过将一个或多个fd传递给select或poll系统调用，阻塞在select操作上，这样select/poll可以帮我们侦测多个fd是否处于就绪状态。select/poll是顺序扫描fd是否就绪，而且支持的fd数量有限，因此它的使用受到了一些制约。Linux还提供了一个epoll系统调用，epoll使用基于事件驱动方式代替顺序扫描，因此性能更高。当有fd就绪时，立即回调函数rollback，如图1-3所示。

（4）信号驱动I/O模型：首先开启套接口信号驱动I/O功能，并通过系统调用sigaction执行一个信号处理函数（此系统调用立即返回，进程继续工作，它是非阻塞的）。当数据准备就绪时，就为该进程生成一个SIGIO信号，通过信号回调通知应用程序调用recvfrom来读取数据，并通知主循环函数处理数据，如图1-4所示。

# 多路复用

Java NIO的核心类库多路复用器Selector就是基于epoll的多路复用技术实现。

在I/O编程过程中，当需要同时处理多个客户端接入请求时，可以利用多线程或者I/O多路复用技术进行处理。I/O多路复用技术通过把多个I/O的阻塞复用到同一个select的阻塞上，从而使得系统在单线程的情况下可以同时处理多个客户端请求。与传统的多线程/多进程模型比，I/O多路复用的最大优势是系统开销小，系统不需要创建新的额外进程或者线程，也不需要维护这些进程和线程的运行，降低了系统的维护工作量，节省了系统资源，I/O多路复用的主要应用场景如下。

◎ 服务器需要同时处理多个处于监听状态或者多个连接状态的套接字；

◎ 服务器需要同时处理多种网络协议的套接字。

目前支持I/O多路复用的系统调用有select、pselect、poll、epoll，在Linux网络编程过程中，很长一段时间都使用select做轮询和网络事件通知，然而select的一些固有缺陷导致了它的应用受到了很大的限制，最终Linux不得不在新的内核版本中寻找select的替代方案，最终选择了epoll。epoll与select的原理比较类似，为了克服select的缺点，epoll作了很多重大改进，现总结如下。

1.支持一个进程打开的socket描述符（FD）不受限制（仅受限于操作系统的最大文件句柄数）。

select最大的缺陷就是单个进程所打开的FD是有一定限制的，它由FD_SETSIZE设置，默认值是1024。对于那些需要支持上万个TCP连接的大型服务器来说显然太少了。可以选择修改这个宏然后重新编译内核，不过这会带来网络效率的下降。我们也可以通过选择多进程的方案（传统的Apache方案）解决这个问题，不过虽然在Linux上创建进程的代价比较小，但仍旧是不可忽视的。另外，进程间的数据交换非常麻烦，对于Java来说，由于没有共享内存，需要通过Socket通信或者其他方式进行数据同步，这带来了额外的性能损耗，增加了程序复杂度，所以也不是一种完美的解决方案。值得庆幸的是，epoll并没有这个限制，它所支持的FD上限是操作系统的最大文件句柄数，这个数字远远大于1024。例如，在1GB内存的机器上大约是10万个句柄左右，具体的值可以通过cat/proc/sys/fs/file-max察看，通常情况下这个值跟系统的内存关系比较大。

2.I/O效率不会随着FD数目的增加而线性下降。

传统select/poll的另一个致命弱点，就是当你拥有一个很大的socket集合时，由于网络延时或者链路空闲，任一时刻只有少部分的socket是“活跃”的，但是select/poll每次调用都会线性扫描全部的集合，导致效率呈现线性下降。epoll不存在这个问题，它只会对“活跃”的socket进行操作——这是因为在内核实现中，epoll是根据每个fd上面的callback函数实现的。那么，只有“活跃”的socket才会去主动调用callback函数，其他idle状态的socket则不会。在这点上，epoll实现了一个伪AIO。针对epoll和select性能对比的benchmark测试表明：如果所有的socket都处于活跃态——例如一个高速LAN环境，epoll并不比select/poll效率高太多；相反，如果过多使用epoll_ctl，效率相比还有稍微地降低。但是一旦使用idle connections模拟WAN环境，epoll的效率就远在select/poll之上了。

3.使用mmap加速内核与用户空间的消息传递。

包括创建一个epoll描述符、添加监听事件、阻塞等待所监听的事件发生、关闭epoll描述符等。

值得说明的是，用来克服select/poll缺点的方法不只有epoll，epoll只是一种Linux的实现方案。在freeBSD下有kqueue，而dev/poll是最古老的Solaris的方案，使用难度依次递增。kqueue是freebsd的宠儿，它实际上是一个功能相当丰富的kernel事件队列，它不仅仅是select/poll的升级，而且可以处理signal、目录结构变化、进程等多种事件。kqueue是边缘触发的。/dev/poll是Solaris的产物，是这一系列高性能API中最早出现的。Kernel提供了一个特殊的设备文件/dev/poll，应用程序打开这个文件得到操作fd_set的句柄，通过写入pollfd来修改它，一个特殊的ioctl调用用来替换select。不过由于出现的年代比较早，所以/dev/poll的接口实现比较原始。


