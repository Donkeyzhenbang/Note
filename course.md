# 计算机网络

## 概念

---

### OSI七层模型

---

![image-20240523154000771](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240523154000771.png)

![image-20240523154046666](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240523154046666.png)

网络通信是进程间通信，网络通信大概分为三个层次
（1）、硬件层：网卡设备，收发网络数据
（2）、驱动层：网卡驱动（Linux 内核网卡驱动代码）内核层
（3）、应用层：上层应用程序（调用 socket 接口或更高级别接口实现网络相关应用程序）

网络互连模型：OSI七层模型 （Open System Interconnection）

![image-20240615120505109](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240615120505109.png)
<img src="https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240615120535102.png" alt="image-20240615120535102"  />

**应用层**

​	应用层（Application Layer）是 OSI 参考模型中的最高层，是最靠近用户的一层，为上层用户提供应用接口，也为用户直接提供各种网络服务。我们常见应用层的网络服务协议有：`HTTP、FTP、TFTP、SMTP、``SNMP、DNS、TELNET、HTTPS、POP3、DHCP。`

**表示层**

​	表示层（Presentation Layer）提供各种用于应用层数据的编码和转换功能，确保一个系统的应用层发送的数据能被另一个系统的应用层识别。如果必要，该层可提供一种标准表示形式，用于将计算机内部的多种数据格式转换成通信中采用的标准表示形式。数据压缩/解压缩和加密/解密（提供网络的安全性）也是表示层可提供的功能之一。

**会话层**

​	会话层（Session Layer）对应主机进程，指本地主机与远程主机正在进行的会话。会话层就是负责建立、管理和终止表示层实体之间的通信会话。该层的通信由不同设备中的应用程序之间的服务请求和响应组成。将不同实体之间表示层的连接称为会话。因此会话层的任务就是组织和协调两个会话进程之间的通信，并对数据交换进行管理。

**传输层**

​	传输层（Transport Layer）定义传输数据的协议端口号，以及端到端的流控和差错校验。该层建立了主机端到端的连接，传输层的作用是为上层协议提供端到端的可靠和透明的数据传输服务，包括差错校验处理和流控等问题。我们通常说的，`TCP、UDP` 协议就工作在这一层，端口号既是这里的“端”。

**网络层**

​	进行逻辑地址寻址，实现不同网络之间的路径选择。本层通过 IP 寻址来建立两个节点之间的连接，为源端发送的数据包选择合适的路由和交换节点，正确无误地按照地址传送给目的端的运输层。网络层（Network Layer）也就是通常说的 IP 层。该层包含的协议有：`IP（Ipv4、Ipv6）、ICMP、IGMP` 等。

**数据链路层**

​	数据链路层（Data Link Layer）是 OSI 参考模型中的第二层，负责建立和管理节点间逻辑连接、进行硬件地址寻址、差错检测等功能。将比特组合成字节进而组合成帧，用 MAC 地址访问介质，错误发现但不能纠正。
​	数据链路层又分为 2 个子层：逻辑链路控制子层（LLC）和媒体访问控制子层（MAC）。MAC 子层的主要任务是解决共享型网络中多用户对信道竞争的问题，完成网络介质的访问控制；LLC 子层的主要任务是建立和维护网络连接，执行差错校验、流量控制和链路控制。
​	数据链路层的具体工作是接收来自物理层的位流形式的数据，并封装成帧，传送到上一层；同样，也将来自上层的数据帧，拆装为位流形式的数据转发到物理层；并且，还负责处理接收端发回的确认帧的信息，以便提供可靠的数据传输。

**物理层**

​	物理层（Physical Layer）是 OSI 参考模型的最低层，物理层的主要功能是：利用传输介质为数据链路层提供物理连接，实现比特流的透明传输，物理层的作用是实现相邻计算机节点之间比特流的透明传送，尽可能屏蔽掉具体传输介质和物理设备的差异。使数据链路层不必考虑网络的具体传输介质是什么。“透明传送比特流”表示经实际电路传送后的比特流没有发生变化，对传送的比特流来说，这个电路好像是看不见的。
​	实际上，网络数据信号的传输是通过物理层实现的，通过物理介质传输比特流。物理层规定了物理设备标准、电平、传输速率等。常用设备有（各种物理设备）集线器、中继器、调制解调器、网线、双绞线、同轴电缆等，这些都是物理层的传输介质.

![image-20240615121907367](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240615121907367.png)

### 数据封装与拆封

---

![image-20240615122103118](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240615122103118.png)

​	当用户发送数据时，将数据向下交给传输层，但是在交给传输层之前，应用层相关协议会对用户数据进行封装，譬如 `MQTT、HTTP` 等协议，其实就是在用户数据前添加一个应用程序头部，这是处于应用层的操作，最后应用层通过调用传输层接口来将封装好的数据交给传输层。
​	传输层会在数据前面加上传输层首部（此处以 TCP 协议为例，图中的传输层首部为 TCP 首部，也可以是 UDP 首部），然后向下交给网络层。
​	同样地，网络层会在数据前面加上网络层首部（IP 首部），然后将数据向下交给链路层，链路层会对数据进行最后一次封装，即在数据前面加上链路层首部（此处使用以太网接口为例，对应以太网首部），然后将数据交给网卡。
​	最后，由网卡硬件设备将数据转换成物理链路上的电平信号，数据就这样被发送到了网络中。这就是网络数据的发送过程，从图中可以看到，各层协议均会对数据进行相应的封装，可以概括为 TCP/IP 模型中的各层协议对数据进行封装的过程。
​	以上便是网络数据的封装过程，当数据被目标主机接收到之后，会进行相反的拆封过程，将每一层的首部进行拆解最终得到用户数据。所以，数据的接收过程与发送过程正好相反，可以概括为 TCP/IP 模型中的各层协议对数据进行解析的过程。

### IP地址

---

​	Internet Protocol Address IP 地址是软件地址，不是硬件地址，硬件 MAC 地址是存储在网卡中的，应用于局域网中寻找目标主机。IP地址的编址方式传统的 IP 地址是一个 32 位二进制数的地址，也叫 IPv4 地址，由 4 个 8 位字段组成。除了 IPv4 之外，还有 IPv6，IPv6 采用 128 位地址长度，8 个 16 位字段组成，在网络通信数据包中，IP 地址以 32 位二进制的形式表示；而在人机交互中，通常使用点分十进制方式表示，譬如 192.168.1.1，这就是点分十进制的表示方式。IP 地址中的 32 位实际上包含 2 部分，分别为网络地址和主机地址，可通过子网掩码来确定网络地址和主机地址分别占用多少位。

![image-20240615123206909](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240615123206909.png)

![image-20240615124747173](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240615124747173.png)

![image-20240615124828461](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240615124828461.png)

![image-20240615125201243](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240615125201243.png)
![image-20240615125217984](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240615125217984.png)

![image-20240615130020540](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240615130020540.png)
![image-20240615130030429](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240615130030429.png)
![image-20240615130509386](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240615130509386.png)

### TCP/IP协议

---

![image-20240615132435860](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240615132435860.png)
![image-20240615132449451](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240615132449451.png)



### TCP协议

---

#### 协议特性

![image-20240615132929193](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240615132929193.png)
![image-20240615133005544](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240615133005544.png)
![image-20240615133025600](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240615133025600.png)

#### 报文格式

![image-20240615133212845](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240615133212845.png)
![image-20240615133235495](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240615133235495.png)
![image-20240615133252319](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240615133252319.png)

#### 建立TCP连接

##### 三次握手

![image-20240615135102243](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240615135102243.png)
![image-20240615135159756](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240615135159756.png)

##### 四次挥手

![image-20240615135314892](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240615135314892.png)
![image-20240615135807771](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240615135807771.png)
![image-20240615135823846](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240615135823846.png)

##### 整体流程

![image-20240615135932406](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240615135932406.png)

#### TCP状态说明

![image-20240615134122651](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240615134122651.png)
![image-20240615134132357](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240615134132357.png)

#### UDP协议

![image-20240615133909573](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240615133909573.png)
![image-20240615133921068](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240615133921068.png)

### 端口号

---

![image-20240615132628686](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240615132628686.png)

### 网卡和路由器

**网卡（Network Interface Card，NIC）**

**网卡的作用和功能：**

1. **连接计算机和网络：** 网卡是计算机与网络进行通信的硬件设备。它负责将计算机连接到局域网（LAN）或广域网（WAN）。
2. **数据帧传输：** 网卡负责在物理层和数据链路层之间传输数据帧。它将数据分组转换为电信号（有线网卡）或无线信号（无线网卡），并通过网络介质传输。
3. **数据封装和解封装：** 网卡在发送数据时，将数据封装成帧；在接收数据时，解封装收到的数据帧，提取数据。
4. **MAC地址管理：** 网卡有一个唯一的物理地址，即MAC地址，用于在局域网中标识设备。

**网卡在联网时的作用：**

- **建立连接：** 通过网卡，计算机可以连接到网络交换机、路由器或其他网络设备，从而进入网络。
- **数据传输：** 网卡负责将计算机的数据通过网络发送到目的设备，并接收来自网络的数据信息。

**路由器（Router）**

**路由器的作用和功能：**

1. **连接不同网络：** 路由器用于连接和隔离不同的网络。它在网络层工作，通过路由表决定数据包的转发路径。
2. **数据包转发：** 路由器根据目的IP地址和路由表，将数据包从一个网络转发到另一个网络。
3. **网络地址转换（NAT）：** 路由器可以进行NAT，将多个私有IP地址映射到一个公共IP地址，允许内部网络设备访问外部网络。
4. **防火墙功能：** 路由器通常具备防火墙功能，能够过滤和管理进出网络的数据流量，增强网络安全。

**路由器在联网时的作用：**

- **数据包转发：** 路由器根据路由表决定如何将数据包从源地址转发到目的地址。
- **网络连接管理：** 路由器管理内部网络和外部网络的连接，确保数据流量按照 预期的路径传输。
- **访问控制：** 路由器可以配置访问控制列表（ACL），限制或允许特定设备和应用程序的网络访问。

**Ping操作**

**Ping的工作机制：**

1. **发送ICMP回显请求：** 当你执行ping命令时，计算机会向目标设备发送ICMP回显请求（Echo Request）。
2. **接收ICMP回显应答：** 目标设备接收到回显请求后，会发送ICMP回显应答（Echo Reply）回到源设备。

**Ping操作依赖的组件：**

- **网卡：** 在执行ping操作时，网卡负责发送和接收ICMP数据包。它将这些数据包通过网络传输。
- **路由器：** 如果ping目标在不同的网络，路由器将负责转发ICMP数据包，确保数据包到达目标设备并返回。

**总结：**

- **网卡负责设备与网络的物理连接和数据传输**。
- **路由器负责不同网络之间的数据转发和管理**。
- **Ping操作主要依赖网卡发送和接收数据包，**
- 网卡和交换机负责局域网内数据通信；路由器负责不同网络间数据传输；
  同一张网卡，经由不同路由器进行网络通信网速也会有差别

## 湖大计网网课

### 第一章 计网概述

1.2因特网概述

![image-20240701222640155](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240701222640155.png)

![image-20240701222812186](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240701222812186.png)

![image-20240701223222686](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240701223222686.png)

![image-20240701224234788](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240701224234788.png)

![image-20240701224803993](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240701224803993.png)

1.3电路交换/分组交换/报文交换

![image-20240702144231103](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240702144231103.png)

![image-20240702144341519](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240702144341519.png)

分组交换（现在主要使用 ） 
分组逐个传输，使得后一个分组的存储和前一个分组的转发可以同时进行
发送方：构造分组/发送分组
路由器：缓存分组/转发分组
接收方：接受分组/还原报文

![image-20240702144617905](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240702144617905.png)

![image-20240702145029977](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240702145029977.png)

![image-20240702145441433](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240702145441433.png)

1.4计网定义和分类
![image-20240702145841994](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240702145841994.png)

![image-20240702150323121](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240702150323121.png)

1.5计网性能指标![image-20240702150635634](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240702150635634.png)

![image-20240702151214930](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240702151214930.png)

![image-20240702151404371](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240702151404371.png)

![image-20240702151720018](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240702151720018.png)

![image-20240702151957054](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240702151957054.png)

![image-20240702152130863](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240702152130863.png)

![image-20240702152240307](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240702152240307.png)

![image-20240702152443806](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240702152443806.png)

![image-20240702152548137](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240702152548137.png)

![image-20240702152726386](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240702152726386.png)

![image-20240702152842686](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240702152842686.png)

![image-20240702152905702](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240702152905702.png)

1.6计网体系结构
1.6.1.常见计网体系结构![image-20240702153508217](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240702153508217.png)

![image-20240702153443869](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240702153443869.png)

1.6.2.计网体系结构分层和必要性![image-20240702154757422](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240702154757422.png)

1.6.3.计网体系结构分层思想举例![image-20240702155026306](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240702155026306.png)

1.6.4.计网体系结构中专用术语
实体/协议/服务![image-20240702160101220](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240702160101220.png)

![image-20240702160051644](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240702160051644.png)

![image-20240702160303421](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240702160303421.png)

![image-20240702160929406](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240702160929406.png)
![image-20240702161016542](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240702161016542.png)

![image-20240702161119885](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240702161119885.png)

![image-20240702161216681](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240702161216681.png)

### 第二章 物理层

2.1物理层基本概念
![image-20240703102049573](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240703102049573.png)

2.2物理层下面的传输媒体
传输媒体不属于计网体系结构中的任何一层
![image-20240703102336026](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240703102336026.png)![image-20240703102452290](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240703102452290.png)![image-20240703102557853](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240703102557853.png)![image-20240703102731571](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240703102731571.png)![image-20240703102805976](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240703102805976.png)

![image-20240703103137867](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240703103137867.png)![image-20240703103119893](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240703103119893.png)![image-20240703103255827](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240703103255827.png)![image-20240703103325772](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240703103325772.png)

![image-20240703103436570](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240703103436570.png)

2.3传输方式![image-20240703103827241](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240703103827241.png)

2.4编码与调制![image-20240703110215315](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240703110215315.png)

 ![image-20240703112647662](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240703112647662.png)

![image-20240703112922374](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240703112922374.png)

2.5信道的极限容量
调制速度即码元传输速度也即波特率

![image-20240703114340641](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240703114340641.png)

![image-20240703114446853](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240703114446853.png)

![image-20240703114509986](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240703114509986.png)

![image-20240703114902744](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240703114902744.png)

![image-20240703115135618](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240703115135618.png)

![image-20240703115146873](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240703115146873.png)

### 第三章 数据链路层 

### 第四章 网络层

### 第五章 运输层

### 第六章 应用层

## socket编程

---

### socket_function

---

![image-20240615140328979](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240615140328979.png)



### socket_problem

```shell
lsof -i:8888 #查看8888端口是否被使用
netstat -tnlp #查看服务器进程使用哪些端口

在agx上实验可以运行 最终发现是防火墙
https://blog.csdn.net/HHHSSD/article/details/117410122

首先将需要连接的端口加到安全组中 例如8888
！！！注意这里查看打开端口号才是正确的 
加完成后将该端口拉入防火墙
1.开启防火墙
systemctl start firewalld
2设置打开的端口号（永久打开）
firewall-cmd --add-port=8000/tcp --permanent
3.更新，端口的设置
firewall-cmd --reload
4.查看已经打开的端口
firewall-cmd --list-all

使用 netstat -tulpn 查看 端口使用情况
# 以8888端口为例
netstat -tulpn | grep 8888
```



# 数据库

## MySQL

```cpp
第一个数据库项目cpp_canteen
项目中需要使用mysql.h需要将MySQL安装目录下的include文件夹和limysql.dll和libmysql.lib添加到根目录下
在Visual Studio中添加项目目录引用include文件夹，此外需要加上这句话
#pragma comment(lib, "libmysql.lib")即可正常运行
另外，程序没连上SQL会发生堆栈错误，这里我们可以先下载SQL WorkBench然后在里面新建类别cpp_canteen
再将对应.sql文件导入对应类别即可 最后别忘记在代码中修改连接的数据库名称和密码
```

## SQLite

```sqlite
常用语句
.help //打印可用的命令清单
.exit .quit //退出数据库系统
.databases //列出数据库名字和所依赖文件
.show //显示各种状态当前值
.table //显示数据库所有表格
 PRAGMA table_info([tablename]);   tablename 为实际的数据表名//查看表具体字段
 sqlite> SELECT * FROM COMPANY; //查询表具体信息
```



# 计算机组成原理

## CSAPP

### 第一章

程序运行流程：
首先我们按下./hello ，字符串从键盘被读取 shell会将读取到的字符串逐一加载到寄存器，处理器会把hello字符串放到内存中。（注意此处不可以DMA直接读取到内存，内存只是存储空间，不放在寄存器里面program count无法指向程序）按下回车键，指令结束，shell会通过一系列指令加载可执行文件hello。这些指令会将hello中的数据和指令从磁盘督导内存而不经过CPU，这既是DMA技术。当hello中的数据和指令到达内存时，处理器就开始执行指令。CPU会将hello world\n 这个字符串复制到寄存器文件，然后再从寄存器文件复制到显示设备。

![image-20240701095309869](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240701095309869.png)

![image-20240701095600280](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240701095600280.png)

![image-20240702105444786](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240702105444786.png)

![image-20240702111734630](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240702111734630.png)

![image-20240702105905118](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240702105905118.png)

![image-20240702110710375](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240702110710375.png)

![image-20240702111326538](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240702111326538.png)

![image-20240702111503634](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240702111503634.png)

![image-20240702111802383](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240702111802383.png)

![image-20240702111905488](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240702111905488.png)

![image-20240702112016418](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240702112016418.png)

![image-20240702112105886](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240702112105886.png)

### 第二章 信息的表示和存储

#### 2.1 信息存储

gcc -m32 -o hello32 hello.c #编译32位程序
gcc -m64 -o hello64 hello.c #编译64位程序
if(a && 5/a) //先判断a是否为0 如果是0，则不再执行后续

![image-20240704100006629](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240704100006629.png)![image-20240704100206456](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240704100206456.png)

算术右移与逻辑右移不同之处：算术右移操作数最高位为1时，左端补1 逻辑右移直接补0
一般有符号数算术右移 无符号数逻辑右移

![image-20240704100617873](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240704100617873.png)

#### 2.2 整数的表示

二进制补码 补码是首位是1则为负，首位为0即是正数

- 正数的补码与原码相同。
- 负数的补码是对原码（符号位除外）按位取反后加1得到的。

![image-20240716155215275](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240716155215275.png)

![image-20240716160847167](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240716160847167.png)

![image-20240716161547701](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240716161547701.png)

![image-20240716161033328](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240716161623476.png)

![image-20240716161706090](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240716161706090.png)

![image-20240716161738855](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240716161738855.png)

![image-20240716161431728](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240716161431728.png)

#### 2.3 整数的运算-remain

正溢出实际上是一种假溢出，因为没有舍弃位，而是最高位由0变为1，产生了负权重
负溢出是真正意义的溢出，最高位产生溢出，而被丢弃，使得负权重丢失，让结果变为正数

![image-20240716163831757](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240716163831757.png)

![image-20240716165402940](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240716165402940.png)

加入偏置后再右移 偏置为（(1<<k )-1）接口得到向0舍入的结果

![image-20240716165824328](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240716165824328.png)

![image-20240716170116721](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240716170116721.png)

#### 2.4 浮点数

![image-20240716175926183](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240716175926183.png)

![image-20240716175215470](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240716175215470.png)

阶码（Exponent）

阶码用于表示浮点数中指数部分的值。它决定了尾数的小数点应该向左或向右移动多少位，从而表示出不同的数值范围。在IEEE 754标准中，阶码通常采用偏移（或称为偏置）的方式来表示，这是为了使得所有的指数都有一个正的表示，包括负数。偏移值通常是2*e*−1−1，其中*e*是阶码的位数。

例如，在一个单精度浮点数（32位）中，阶码占据了其中的8位（包括一个符号位），因此偏移值为27−1=127。如果一个单精度浮点数的阶码字段为128（二进制为10000000），那么实际的指数值为128−127=1，表示小数点向右移动1位。

尾数（Mantissa 或 Significand）

尾数（或称为有效数字）是浮点数中表示数值精度（或大小）的部分。它紧跟在阶码之后，包含了浮点数中除去隐含的整数位（在IEEE 754标准中，对于非零的规格化浮点数，尾数前会隐含一个1，这个1不会存储在尾数字段中）之外的所有位。尾数可以是整数也可以是分数，但在计算机中，它们通常以二进制形式存储。

在IEEE 754标准中，尾数部分还包括了一个隐含的二进制点，这意味着尾数实际上是一个二进制小数，而不仅仅是整数或二进制数的集合。这种设计允许浮点数以更高的精度来表示小数。

![image-20240716180332384](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240716180332384.png)

![image-20240716180809606](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240716180809606.png)   

浮点数结果不具有整数相关性![image-20240716181450431](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240716181450431.png)

### 第三章 程序的机器级表示



# 操作系统