4、`ps aux| grep java` 查看到当前java进程使用cpu、内存、磁盘的情况，获取使用量异常的进程
5、`top -Hp 进程pid` 检查当前使用异常线程的pid
6、把线程tid变为16进制如 31695 - 》 7bcf 然后得到0x7bcf `printf "%x\n" ti
7、`jstack 进程的pid|grep -A20 0x7bcf` 得到相关进程的代码

### **一.简介**

最近一段时间 某台[服务器](https://cloud.tencent.com/act/pro/promotion-cvm?from_column=20065&from=20065)上的一个应用总是隔一段时间就自己挂掉 用top看了看 从重新部署应用开始没有多长时间CPU占用上升得很快

排查步骤

1.使用top 定位到占用CPU高的进程PID `top`

2.通过ps aux | grep PID命令 获取线程信息，并找到占用CPU高的线程 `ps -mp pid -o THREAD,tid,time | sort -rn`

3.将需要的线程ID转换为16进制格式 `printf "%x\n" tid`

4.打印线程的堆栈信息 到了这一步具体看堆栈的日志来定位问题了 `jstack pid |grep tid -A 30`

### **二.例子**

1.`top` 可以看出PID 733进程 的占用CPU 172%

![](https://ask.qcloudimg.com/http-save/7250484/ig1picpkrr.png)

2.查找进程733下的线程 可以看到TID 线程775占用了96%且持有了很长时间 其实到这一步基本上能猜测到应该是 肯定是那段代码发生了死循环 `ps -mp 733 -o THREAD,tid,time | sort -rn`

![](https://ask.qcloudimg.com/http-save/7250484/xbuz4f26ci.png)

3.线程ID转换为16进制格式 `printf "%x\n" 775`

![](https://ask.qcloudimg.com/http-save/7250484/nfd7nz6hdf.png)

4.查看java的堆栈信息，打印30行 `jstack -l pid号 |grep 0x线程号 -A 30`

![](https://ask.qcloudimg.com/http-save/7250484/oss56r63pw.png)

![](https://ask.qcloudimg.com/http-save/7250484/5w4kjcy5qp.png)

