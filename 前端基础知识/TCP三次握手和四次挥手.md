## TCP三次握手和四次挥手

### TCP基础知识

- TCP（Transmission Control Protocol，传输控制协议）是一种**面向连接的、可靠的、基于字节流的通信协议**，数据在传输前要建立连接，传输完毕后还要断开连接。
- 客户端在收发数据前要使用 connect() 函数和服务器建立连接。建立连接的目的是保证**IP地址、端口、物理链路**等正确无误，为数据的传输开辟通道。
- TCP建立连接时要传输三个数据包，俗称三次握手（Three-way Handshaking）



### TCP数据报结构

- 序号：**Seq**（Sequence Number）序号占32位，用来标识从计算机A发送到计算机B的数据包的序号，计算机发送数据时对此进行标记。
- 确认号：**Ack**（Acknowledge Number）确认号占32位，客户端和服务器端都可以发送，**Ack = Seq + 1**。
- 标志位：每个标志位占用1Bit，共有6个，分别为 URG、**ACK**、PSH、RST、**SYN**、**FIN**，具体含义如下：
  - URG：紧急指针（urgent pointer）有效
  - **ACK：确认序号有效**
  - PSH：接收方应该尽快将这个报文交给应用层
  - RST：重置连接
  - **SYN：建立一个新连接**
  - **FIN：断开一个连接**

如下图所示：

![]( https://img-blog.csdnimg.cn/20200319132644853.png)



### TCP的三次握手

1. 首先 Client 端发送连接请求报文

2. Server 段接受连接后回复 ACK 报文，并为这次连接分配资源。

3. Client 端接收到 ACK 报文后也向 Server 段发生 ACK 报文，并分配资源，这样 TCP 连接就建立了。

![]( https://img-blog.csdnimg.cn/2020031913392621.png)



### 为什么需要三次握手

只有经过三次握手才能确认双发的收发功能都正常，缺一不可：

- 第一次握手（客户端发送 SYN 报文给服务器，服务器接收该报文）：客户端什么都不能确认；服务器确认了对方发送正常，自己接收正常
- 第二次握手（服务器响应 SYN 报文给客户端，客户端接收该报文）：客户端确认了：自己发送、接收正常，对方发送、接收正常；服务器确认了：对方发送正常，自己接收正常
- 第三次握手（客户端发送 ACK 报文给服务器）：客户端确认了：自己发送、接收正常，对方发送、接收正常； 服务器确认了：自己发送、接收正常，对方发送、接收正常

> 三次握手的目的是建立可靠的通信信道，说到通讯，简单来说就是数据的发送与接收，而三次握手最主要的目的就是**双方确认自己与对方的发送与接收是正常的**。



### TCP的四次挥手

1. Client发送一个FIN，用来关闭Client到Server的数据传送，Client进入FIN_WAIT_1状态。

2. Server收到FIN后，发送一个ACK给Client，Server进入CLOSE_WAIT状态。

3. Server发送一个FIN，用来关闭Server到Client的数据传送，Server进入LAST_ACK状态。

4. Client收到FIN后，Client进入TIME_WAIT状态，发送ACK给Server，Server进入CLOSED状态，完成四次握手。

![]( https://img-blog.csdnimg.cn/20200319135917416.png)