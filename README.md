@[TOC](图解HTTP)
# [本人的Github地址](https://github.com/ftllove)
# TCP/IP协议

> 通常使用的网络（包括互联网）是在 TCP/IP 协议族的基础上运作的。
而 HTTP 属于它内部的一个**子集**。

![TCP/IP 是互联网相关的各类协议族的总称](https://img-blog.csdnimg.cn/b0db09a33f6e472388ece70a3660119d.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_10,text_Q1NETiBARlRMX0RZQw==,size_20,color_FFFFFF,t_70,g_se,x_16)
# TCP/IP的分层
> TCP/IP 协议族按层次分别分 为以下 4 层：***应用层、传输层、网络层和数据链路层***
> 把 TCP/IP 层次化是有好处的。比如，如果互联网只由一个协议统筹，某个地方需要改变设计时，就必须把所有部分整体替换掉。而分层之后只需把变动的层替换掉即可。把各层之间的接口部分规划好之后，每个层次内部的设计就能够自由改动了。 
> 值得一提的是，层次化之后，设计也变得相对简单了。处于应用层上的应用可以只考虑分派给自己的任务，而不需要弄清对方在地球上哪个地方、对方的传输路线是怎样的、是否能确保传输送达等问题。

## TCP/IP各层的作用
----
 1. 应用层
 > 应用层决定了向用户提供应用服务时通信的活动。 
 > TCP/IP 协议族内预存了各类通用的应用服务。
 > 比如，FTP（File Transfer Protocol，文件传输协议）和 DNS（Domain Name System，域名系统）服务就是其中两类。 
 > **HTTP 协议也处于该层。**
 2. 传输层
 > 传输层对上层应用层，提供处于网络连接中的两台计算机之间的数据传输。
 > 在传输层有两个性质不同的协议：**TCP**（Transmission Control Protocol，传输控制协议）和 **UDP**（User Data Protocol，用户数据报协议）。
 3. 网络层
 > 网络层用来处理在网络上流动的数据包。
 > 数据包是网络传输的最小数据单位。***该层规定了通过怎样的路径/传输路线到达对方计算机，并把数据包传送给对方。***
 > 和对方计算机之间通过多台计算机或网络设备进行传输时，网络层就是在多条通道中，选择其中一条路线进行传输。
 4. 数据链路层（硬件）
 > 用来处理连接网络的硬件部分。
 > 包括控制操作系统、硬件的设备驱动、NIC（Network Interface Card，网络适配器，即网卡），及光纤等物理可见部分（还包括连接器等一切传输媒介）。
 > 硬件上的范畴均在链路层的作用范围之内。

----

#  TCP/IP 通信传输流
![传输流图
](https://img-blog.csdnimg.cn/6d965b092660462db6f78a48398a0d91.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBARlRMX0RZQw==,size_20,color_FFFFFF,t_70,g_se,x_16)
> 利用TCP/IP协议族进行网络通信时，会通过分层顺序与对方进行通信。
> 发送端从应用层往下走，接收端则从数据链路层往上走。

```
HTTP举例来说明，首先作为发送端的客户端在应用层发出一个想看某个Web 页面的HTTP请求。
接着，为了传输方便，在传输层（TCP 协议）把从应用层处收到的数据（HTTP 请求报文）进行分割，并在各个报文上打上标记序号及端口号后转发给网络层。
在网络层（IP 协议），增加作为通信目的地的MAC地址后转发给数据链路层。这样一来，发往网络的通信请求就准备齐全了。
接收端的服务器在链路层接收到数据，按序往上层发送，一直到应用层。当传输到应用层，才能算真正接收到由客户端发送过来的HTTP请求。
```
![TCP/IP封装](https://img-blog.csdnimg.cn/f84eeba5cbe54fa9aa240ed7bc42c84e.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBARlRMX0RZQw==,size_20,color_FFFFFF,t_70,g_se,x_16)
> 发送端在层与层之间传输数据时，每经过一层时必定会被打上一个该层所属的首部信息。反之，接收端在层与层传输数据时，每经过一层时会把对应的首部去除掉。 这种把数据信息包装起来的做法称为封装（encapsulate）。
# IP 、TCP和 DNS
###  负责传输的 IP 协议
> 按层次分，IP（Internet Protocol）网际协议位于网络层。Internet Protocol 这个名称可能听起来有点夸张，但事实正是如此，因为几乎 所有使用网络的系统都会用到 IP 协议。TCP/IP 协议族中的 IP 指的就 是网际协议，协议名称中占据了一半位置，其重要性可见一斑。“IP”其实是一种协议的名称。
> IP 协议的作用是把各种数据包传送给对方。而要保证确实传送到对方 那里，则需要满足各类条件。其中两个重要的条件是 IP 地址和 MAC 地址。
> IP 地址指明了节点被分配到的地址，MAC 地址是指网卡所属的固定 地址。IP 地址可以和 MAC 地址进行配对。IP 地址可变换，但 MAC 地址基本上不会更改。


> 使用 ARP 协议凭借 MAC 地址进行通信!!!
> IP 间的通信依赖 MAC 地址。在网络上，通信的双方在同一局域网 （LAN）内的情况是很少的，通常是经过多台计算机和网络设备中转 才能连接到对方。而在进行中转时，会利用下一站中转设备的 MAC 地址来搜索下一个中转目标。这时，会采用 ARP 协议（Address Resolution Protocol）。ARP 是一种用以解析地址的协议，根据通信方 的 IP 地址就可以反查出对应的 MAC 地址。
![在这里插入图片描述](https://img-blog.csdnimg.cn/48c911ad603846d08e4263be708327f1.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBARlRMX0RZQw==,size_20,color_FFFFFF,t_70,g_se,x_16)

### 确保可靠性的 TCP 协议
> 按层次分，TCP 位于传输层，提供可靠的字节流服务。 
> 所谓的字节流服务（Byte Stream Service）是指，为了方便传输，将大 块数据分割成以报文段（segment）为单位的数据包进行管理。而可 靠的传输服务是指，能够把数据准确可靠地传给对方。一言以蔽之， TCP 协议为了更容易传送大数据才把数据分割，而且 TCP 协议能够 确认数据最终是否送达到对方。
确保数据能到达目标 为了准确无误地将数据送达目标处，TCP 协议采用了三次握手 （three-way handshaking）策略。用 TCP 协议把数据包送出去后，TCP 不会对传送后的情况置之不理，它一定会向对方确认是否成功送达。握手过程中使用了 TCP 的标志（flag） —— SYN（synchronize） 和 ACK（acknowledgement）。
发送端首先发送一个带 SYN 标志的数据包给对方。接收端收到后， 回传一个带有 SYN/ACK 标志的数据包以示传达确认信息。最后，发送端再回传一个带 ACK 标志的数据包，代表“握手”结束。
若在握手过程中某个阶段莫名中断，TCP 协议会再次以相同的顺序发 送相同的数据包。

![在这里插入图片描述](https://img-blog.csdnimg.cn/90ab212de4424b6f9613d5c986b5d0dd.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBARlRMX0RZQw==,size_20,color_FFFFFF,t_70,g_se,x_16)


**未完待续...**
