- 一般来说我们总是希望数据传输的更快一些
	但如果发送方把数据发送的过快，接收方就可能来不及接收，这就会造成数据的丢失
- 所谓流量控制就是*让发送方的发送速率不要太快，要让接收方来得及接收。*
- 利用*滑动窗口机制*可以很方便地在tcp连接上实现对发送方的流量控制
我们来举例说明
假设主机a和b是英特网上的两台主机
他们之间已经建立了tcp连接
a给b发送数据
b对a进行流量控制
这是主机a装待发送数据的字节序号
假设主机a发送的每个tcp报文段可携带100字节数据
因此图中每个小格子表示100个字节数据的序号
在主机a和b建立tcp连接时
b告诉a我的接收窗口为400
因此主机a将自己的发送窗口也设置为400
这意味着主机a在未收到主机b发来的确认时
可将序号落入发送窗口中的全部数据发送出去
接下来我们举例说明主机b对a的流量控制
主机a将发送窗口内序号1~100的数据封装成一个tcp报文段
发送出去
发送窗口内还有300字节可以发送
这里的s e q是tcp报文段首部中的序号字段取值
一表示tcp报文段数据载荷的第一个字节的序号是一
这里的data表示这是tcp数据报文段
主机a将发送窗口内序号101~200的数据封装成一个tcp报文段
发送出去
发送窗口内还有200字节可以发送
主机a将发送窗口内序号201~300的数据封装成一个tcp报文段
发送出去
但该报文段在传输过程中丢失了
主机a发送窗口内还有100字节可以发送
主机b对主机a所发送的201号以前的数据进行累计确认
并在该累计确认中将窗口字段的值调整为300
也就是对主机a进行流量控制
这里的大写a c k是tcp报文段首部中的标志位
取值一表示这是一个tcp确认报文段
小写a c k是tcp报文段首部中的确认号
字段取值201表示序号201之前的数据已全部正确接收
现在希望收到序号201及其后续数据
rw n d是tcp报文段首部中的窗口字段
取值300
表示自己的接收窗口大小为300
主机a收到该累计确认后
将发送窗口向前滑动
使已发送并收到确认的这些数据的序号移出发送窗口
由于主机b在该累计确认中将自己的接收窗口调整为了300
因此主机a相应地将自己的发送窗口调整为300
目前主机a发送窗口内的序号为201~500
也就是主机a还可以发送这300字节
其中201~300号字节是已发送的数据
若重传计时器超时
它们会被重传301~400号字节
以及401号到5号字节还未被发送
可被分别封装在一个tcp报文段中
发送主机a现在可将发送缓存中序号1~200的字节数据全部删除了
因为已经收到了主机币对他们的累计确认
主机a将发送窗口内序号301~400的数据封装成一个tcp报文段
发送出去
发送窗口内还有100字节可以发送
主机a将发送窗口内序号401~500的数据封装成一个tcp报文段
发送出去
至此序号落在发送窗口内的数据已经全部发送出去了
不能再发送新数据了
现在发送窗口内序号201~300
这100个字节数据的重传计时器超时了
主机a将它们重新封装成一个tcp报文段发送出去
暂时不能发送其他数据
主机b收到该重传的tcp报文段后
对主机a所发送的501号以前的数据进行累计确认
并在该累计确认中将窗口字段的值调整为100
这是主机b对主机a进行的第二次流量控制
主机a收到该累计确认后
将发送窗口向前滑动
使已发送并收到确认的这些数据的序号移出发送窗口
由于主题b在该累计确认中将自己的接收窗口调整为了100
因此主机a相应地将自己的发送窗口调整为100
目前主机a发动窗口那个序号为501~600
也就是主机a还可以发送这100字节
主机a现在可将发送缓存中
序号201~500的字节数据全部删除了
因为已经收到了主机地对他们的累计确认
主机a将发送窗口内序号501~600的数据封装成一个tcp报文段
发送出去
至此序号落在发送窗口内的数据已经全部发送出去了
不能再发送新数据了
主机b对主机a所发送的601号以前的数据进行累计确认
并在该累计确认中将窗口字段的值调整为零
这是主机b对主机a进行的第三次流量控制
主机a收到该累计确认后
将发送窗口向前滑动
使已发送并收到确认的这些数据的序号移出发送窗口
由于主机b在该累计确认中将自己的接收窗口调整为了零
因此主机a相应的将自己的发送窗口调整为零
目前主机a不能再发送一般的tcp报文段了
主机a现在可将发送缓存中
序号501~600的字节数据全部删除了
因为已经收到了主机b对他们的累计确认
假设主机b向主机a发送了零窗口的报文段
后不久主机b的接收缓存又有了一些存储空间
于是主机b向主机a发送了接收窗口等于300的报文段
然而这个报文段在传输过程中丢失了
主机a一直等待主机b发送的非零窗口的通知
而主机b也一直等待主机a发送的数据
如果不采取措施
这种互相等待而形成的思索局面将一直持续下去
为了解决这个问题
tcp为每一个连接设有一个持续计时器
只要tcp连接了一方收到对方的零窗口通知
就要启动持续计时器
若持续计时器超时
就要发送一个零窗口探测报文
仅携带一字节的数据
而对方在确认这个探测报文段时
给出自己现在的接收窗口值
如果接收窗口仍然是零
那么收到这个报文段的一方就要重新启动持续计时器
如果接收窗口不是零
那么死锁的截面就可以被打破了
在本地周主机a收到零窗口通知时
就要启动一个持续计时器
当直接计时器超时
主机a立刻发送一个仅携带一字节数据的零窗口探测报文段
假设主机b此时的接收窗口又为零了
主机b就在确认这个零窗口探测报文段时
给出自己现在的接收窗口值为零
主机a再次收到零窗口通知
就再次启动一个持续计时器
当持续计时器超时
主机a立刻发送一个零窗口探测报文段
假设主机b此时的接收缓存又有了一些存储空间
于是将自己的接收窗口调整为了300
主机b就在确认这个零窗口探测报文段时
给出自己现在的接收窗口值为300
这样就打破了死锁的局面
同学们可能会有这样的疑问
主机a所发送的零窗口探测报文段到达主机b时
如果主机b此时的接收窗口仍然为零
那么主机b根本就无法接受该报文段
又怎么会针对该报文段给主机a发回确认
实际上tcp规定
即使接收窗口为零
也必须接受零窗口探测报文段
确认报文段以及携带有紧急数据的报文段
请大家再来思考一下这个问题
如果零窗口探测报文段丢失了
会出现怎样的问题呢
还能否打破死锁的局面呢
回答是肯定的
因为零窗口探测报文段也有重传计时器
当重传计时器超时后
零窗口探测报文段会被重传
接下来我们来做一个相关的练习题
这是计算机专业考研全国统考计算机网络部分2010年的题
39
由于该题涉及到了我们还未学习的ui控制的内容
因此我们需要先做一些说明
然后再请同学们来完成tcp发送方的发送窗口
实际上是在自身的拥塞窗口和tcp接收方的接收窗口中取小者
而在我们本节课的举例中
为了简单起见
我们忽略了拥塞控制
也就是认为tcp发送方的发送窗口等于接收方的接收窗口
本题未给出tcp发送方的发送窗口的初始值
则我们取英塞窗口值作为发送窗口值
现在请大家暂停播放视频
思考一下
给出你的答案
答案是选项a
大家都选对了吗
我们来一起画图分析一下
这是主机甲装带发送数据的序号
根据题意可知
主机甲的发送窗口为4000字节
主机甲向主机已连续发送两个最大段
共2000字节
也就是将发送窗口内序号0~1999的字节数据发送出去
主机乙给主机甲发送针对第一个段的确认
并在该确认中给出自己当前的接收窗口大小为2000字节
主机甲受到该确认后
将发动窗口向前滑动
使已发送并收到确认的第一个段的序号移出发送窗口
由于主机已在确认中给出的自己的接收窗口大小为2000字节
因此主机甲相应地将自己的发送窗口调整为2000字节
很显然主机甲还可以向主机已发送2000~2999号字节数据
共1000个字节
本节课到这里就结束了
我们将本节课的内容小结如下
同学们
我们下节课再见
