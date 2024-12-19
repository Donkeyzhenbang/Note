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



## Linux高性能服务器编程

### 第一篇 TCP/IP协议族

#### 第一章 TCP/IP协议族

##### TCP/IP协议族体系结构及主要协议

![image-20241105093533877](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20241105093533877.png)

**数据链路层**
	1.<font color="red">数据链路层实现了网卡接口的网络驱动程序</font>，**以处理数据在物理媒介（比如以太网、令牌环等）上的传输。不同的物理网络具有不同的电气特性，网络驱动程序隐藏了这些细节，为上层协议提供一个统一的接口。**

​	**2.数据链路层两个常用的协议是ARP协议（Address Resolve Protocol，地址解析协议）和RARP协议（Reverse Address Resolve Protocol，逆地址解析协议）。它们实现了IP地址和机器物理地址（通常是MAC地址，以太网、令牌环和802.11无线网络都使用MAC地址）之间的相互转换。**

​	**3.网络层使用IP地址寻址一台机器，而数据链路层使用物理地址寻址一台机器，因此网络层必须先将目标机器的IP地址转化成其物理地址，才能使用数据链路层提供的服务，这就是ARP协议的用途。**RARP协议仅用于网络上的某些无盘工作站。因为缺乏存储设备，无盘工作站无法记住自己的IP地址，但它们可以利用网卡上的物理地址来向网络管理者（服务器或网络管理软件）查询自身的IP地址。运行RARP服务的网络管理者通常存有该网络上所有机器的物理地址到IP地址的映射  

**网络层**
	**1.网络层实现数据包的选路和转发。**WAN（Wide Area Network，广域网）通常使用众多分级的路由器来连接分散的主机或LAN（Local Area Network，局域网），因此，通信的两台主机一般不是直接相连的，而是通过多个中间节点（路由器）连接的。网络层的任务就是选择这些中间节点，以确定两台主机之间的通信路径。同时，网络层对上层协议隐藏了网络拓扑连接的细节，使得在传输层和网络应用程序看来，通信的双方是直接相连的。 

​	2.**网络层最核心的协议是IP协议（Internet Protocol，因特网协议）。IP协议根据数据包的目的IP地址来决定如何投递它。如果数据包不能直接发送给目标主机，那么IP协议就为它寻找一个合适的下一跳（next hop）路由器，并将数据包交付给该路由器来转发。多次重复这一过程，数据包最终到达目标主机，或者由于发送失败而被丢弃。可见， IP协议使用逐跳（hop by hop）的方式确定通信路径。**我们将在第2章详细讨论IP协议。

​	3.网络层另外一个重要的协议是ICMP协议（Internet Control Message Protocol，因特网控制报文协议）。它是IP协议的重要补充，主要用于**检测网络连接**。  8位类型字段用于区分报文类型。它将ICMP报文分为两大类：一类是差错报文，这类报文主要用来回应网络错误，比如目标不可到达（类型值为3）和重定向（类型值为5）；另一类是查询报文，这类报文用来查询网络信息，比如ping程序就是使用ICMP报文查看目标是否可到达（类型值为8）的。有的ICMP报文还使用8位代码字段来进一步细分不同的条件。比如重定向报文使用代码值0表示对网络重定向，代码值1表示对主机重定向。ICMP报文使用16位校验和字段对整个报文（包括头部和内容部分）进行循环冗余校验（Cyclic Redundancy Check，CRC），以检验报文在传输过程中是否损坏。不同的ICMP报文类型具有不同的正文内容。我们将在第2章详细讨论主机重定向报文，其他ICMP报文格式请参考ICMP协议的标准文档RFC 792。

​	4.ICMP协议并非严格意义上的网络层协议，因为它使用处于同一层的IP协议提供的服务（一般来说，上层协议使用下层协议提供的服务）。  

​	5.telnet是23端口，ssh是22端口，那么ping是什么端口? 答：ping命令是基于ICMP，是在网络层。而端口号，是传输层的内容。所以在ICMP中根本就不关注端口号这样的信息。

![image-20241105100006449](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20241105100006449.png)

**传输层**
	1.ICMP协议并非严格意义上的网络层协议，因为它使用处于同一层的IP协议提供的服务（一般来说，上层协议使用下层协议提供的服务）。 

​	2.垂直的实线箭头表示TCP/IP协议族各层之间的实体通信（数据包确实是沿着这些线路传递的），而水平的虚线箭头表示逻辑通信线路。该图中还附带描述了不同物理网络的连接方法。可见，**数据链路层（驱动程序）封装了物理网络的电气细节；网络层封装了网络连接的细节；传输层则为应用程序封装了一条端到端的逻辑通信链路，它负责数据的收发、链路的超时重连等。传输层协议主要有三个：TCP协议、UDP协议和SCTP协议。**

​	3.**TCP协议（Transmission Control Protocol，传输控制协议）为应用层提供可靠的、面向连接的和基于流（stream）的服务。**TCP协议使用**超时重传、数据确认**等方式来确保数据包被正确地发送至目的端，因此TCP服务是可靠的。使用TCP协议通信的双方必须先建立TCP连接，并在内核中为该连接维持一些必要的数据结构，比如连接的状态、读写缓冲区，以及诸多定时器等。当通信结束时，双方必须关闭连接以释放这些内核数据。**TCP服务是基于流的。基于流的数据没有边界（长度）限制，它源源不断地从通信的一端流入另一端。发送端可以逐个字节地向数据流中写入数据，接收端也可以逐个字节地将它们读出。**

​	4.**UDP协议（User Datagram Protocol，用户数据报协议）**则与TCP协议完全相反，它为应用层提供**不可靠、无连接和基于数据报**的服务。“不可靠”意味着UDP协议无法保证数据从发送端正确地传送到目的端。如果数据在中途丢失，或者目的端通过数据校验发现数据错误而将其丢弃，则UDP协议只是简单地通知应用程序发送失败。因此，**使用UDP协议的应用程序通常要自己处理数据确认、超时重传等逻辑。UDP协议是无连接的，即通信双方不保持一个长久的联系，因此应用程序每次发送数据都要明确指定接收端的地址（IP地址等信息）。基于数据报的服务，是相对基于流的服务而言的。每个UDP数据报都有一个长度，接收端必须以该长度为最小单位将其所有内容一次性读出，否则数据将被截断。**  
​	

![image-20241105100540693](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20241105100540693.png)

**应用层**
	1.应用层负责处理应用程序的逻辑。数据链路层、网络层和传输层负责处理网络通信细节，这部分必须既稳定又高效，因此它们都在内核空间中实现，如图1-1所示。而应用层则在用户空间实现，因为它负责处理众多逻辑，比如文件传输、名称查询和网络管理等。如果应用层也在内核中实现，则会使内核变得非常庞大。当然，也有少数服务器程序是在内核中实现的，这样代码就无须在用户空间和内核空间来回切换（主要是数据的复制），极大地提高了工作效率。不过这种代码实现起来较复杂，不够灵活，且不便于移植。

​	2.ping是应用程序，而不是协议，前面说过它利用ICMP报文检测网络连接，是调试网络环境的必备工具。

​	3.telnet协议是一种远程登录协议，它使我们能在本地完成远程任务，本书后续章节将会多次使用telnet客户端登录到其他服务上。

​	4.OSPF（Open Shortest Path First，开放最短路径优先）协议是一种动态路由更新协议，用于路由器之间的通信，以告知对方各自的路由信息。

​	5.DNS（Domain Name Service，域名服务）协议提供机器域名到IP地址的转换，我们将在后面简要介绍DNS协议。

​	6.应用层协议（或程序）可能跳过传输层直接使用网络层提供的服务，比如ping程序和OSPF协议。应用层协议（或程序）通常既可以使用TCP服务，又可以使用UDP服务，比如DNS协议。我们可以通过/etc/services文件查看所有知名的应用层协议，以及它们都能使用哪些传输层服务。

##### 封装

​	1.上层协议是如何使用下层协议提供的服务的呢？其实这是通过封装（encapsulation）实现的。应用程序数据在发送到物理网络上之前，将沿着协议栈从上往下依次传递。每层协议都将在上层数据的基础上加上自己的头部信息（有时还包括尾部信息），以实现该层的功能，这个过程就称为封装.  

![image-20241105102104036](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20241105102104036.png)

​	2.经过TCP封装后的数据称为TCP报文段（TCP message segment），或者简称TCP段。前文提到，TCP协议为通信双方维持一个连接，并且在内核中存储相关数据。这部分数据中的TCP头部信息和TCP内核缓冲区（发送缓冲区或接收缓冲区）数据一起构成了TCP报文段，如图1-5中的虚线框所示。

![image-20241105102116094](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20241105102116094.png)

​	3.当发送端应用程序使用send（或者write）函数向一个TCP连接写入数据时，内核中的TCP模块首先把这些数据复制到与该连接对应的TCP内核发送缓冲区中，然后TCP模块调用IP模块提供的服务，传递的参数包括TCP头部信息和TCP发送缓冲区中的数据，即TCP报文段。关于TCP报文段头部的细节，我们将在第3章讨论。

​	4.**经过UDP封装后的数据称为UDP数据报（UDP datagram）**。**UDP对应用程序数据的封装与TCP类似。不同的是，UDP无须为应用层数据保存副本，因为它提供的服务是不可靠的。当一个UDP数据报被成功发送之后，UDP内核缓冲区中的该数据报就被丢弃了。如果应用程序检测到该数据报未能被接收端正确接收，并打算重发这个数据报，则应用程序需要重新从用户空间将该数据报拷贝到UDP内核发送缓冲区中。**

​	5.**经过IP封装后的数据称为IP数据报（IP datagram）**。**IP数据报也包括头部信息和数据部分，其中数据部分就是一个TCP报文段、UDP数据报或者ICMP报文。**我们将在第2章详细讨论IP数据报的头部信息。

​	6.**经过数据链路层封装的数据称为帧（frame）**。传输媒介不同，帧的类型也不同。比如，以太网上传输的是以太网帧（ethernet frame），而令牌环网络上传输的则是令牌环帧（token ring frame）。以以太网帧为例，其封装格式如图1-6所示。

![image-20241105102253384](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20241105102253384.png)

​	7.以太网帧使用6字节的目的物理地址和6字节的源物理地址来表示通信的双方。关于类型（type）字段，我们将在后面讨论。4字节CRC字段对帧的其他部分提供循环冗余校验。**帧的最大传输单元（Max Transmit Unit，MTU），即帧最多能携带多少上层协议数据（比如IP数据报），通常受到网络类型的限制**。图1-6所示的以太网帧的MTU是1500字节。正因为如此，过长的IP数据报可能需要被分片（fragment）传输。**帧才是最终在物理网络上传送的字节序列**。至此，封装过程完成。

##### 分用

​	当帧到达目的主机时，将沿着协议栈自底向上依次传递。各层协议依次处理帧中本层负责的头部数据，以获取所需的信息，并最终将处理后的帧交给目标应用程序。这个过程称为分用（demultiplexing）。分用是依靠头部信息中的类型字段实现的。标准文档RFC 1700定义了所有标识上层协议的类型字段以及每个上层协议对应的数值。图1-7显示了以太网帧的分用过程。  

![](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20241105113001636.png)

​	因为IP协议、ARP协议和RARP协议都使用帧传输数据，所以帧的头部需要提供某个字段（具体情况取决于帧的类型）来区分它们。以以太网帧为例，它使用2字节的类型字段来标识上层协议（见图1-6）。如果主机接收到的以太网帧类型字段的值为0x800，则帧的数据部分为IP数据报（见图1-4），以太网驱动程序就将帧交付给IP模块；若类型字段的值为0x806，则帧的数据部分为ARP请求或应答报文，以太网驱动程序就将帧交付给ARP模块；若类型字段的值为0x835，则帧的数据部分为RARP请求或应答报文，以太网驱动程序就将帧交付给RARP模块。

​	同样，因为ICMP协议、TCP协议和UDP协议都使用IP协议，所以IP数据报的头部采用16位的协议（protocol）字段来区分它们。

​	TCP报文段和UDP数据报则通过其头部中的16位的端口号（port number）字段来区分上层应用程序。比如DNS协议对应的端口号是53，HTTP协议（Hyper-Text Transfer Protocol，超文本传送协议）对应的端口号是80。所有知名应用层协议使用的端口号都可在/etc/services文件中找到。

​	帧通过上述分用步骤后，最终将封装前的原始数据送至目标服务（图1-7中的ARP服务、RARP服务、ICMP服务或者应用程序）。这样，在顶层目标服务看来，封装和分用似乎没有发生过。

##### 测试网络

![image-20241105113238137](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20241105113238137.png)

​	该测试网络主要用于分析ARP协议、IP协议、ICMP协议、TCP协议和DNS协议。我们通过抓取该网络上的以太网帧，查看其中的以太网帧头部、IP数据报头部、TCP报文段头部信息，以获取网络通信的细节。这样，以理论结合实践，我们就清楚TCP/IP通信具体是如何进行的了。作者编写的多个客户端、服务器程序都是使用该网络来调试和测试的 。

​	对于路由器，我们仅列出了其LAN网络IP地址（192.168.1.1），而忽略了ISP（Internet Service Provider，因特网服务提供商）给它分配的WAN网络IP地址  

##### ARP协议工作原理

- ARP 协议的 **传输和广播** 发生在数据链路层。
- **ARP 的调用和需求** 来自网络层，因为 IP 地址到 MAC 地址的映射是网络层通信所需要的。
- 虽然**数据链路层并不直接“拥有” IP 地址**，但通过操作系统的跨层共享机制，数据链路层的 ARP 模块可以访问本机的 IP 地址，从而实现 IP 地址到 MAC 地址的转换和匹配功能。这种跨层信息共享是为了 ARP 协议的运行需求设计的。

![image-20241105155740681](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20241105155740681.png)

​	ARP协议能实现任意网络层地址到任意物理地址的转换，不过本书仅讨论从IP地址到以太网地址（MAC地址）的转换。其工作原理是：主机向自己所在的网络广播一个ARP请求，该请求包含目标机器的网络地址。此网络上的其他机器都将收到这个请求，但只有被请求的目标机器会回应一个ARP应答，其中包含自己的物理地址 。

![image-20241105154103140](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20241105154103140.png)

​	在网络中数据包传输也是如此，源IP地址和和目标IP地址在传输过程中是不会变化的（前提没有使用NAT网络），只有源MAC地址和目标MAC一直在变化。

​	IP作用时主机之间通信用的，而MAC作用时实现直连的两个设备之间通信，而IP负责在没有直连的两个网络之间进行通信传输。

![image-20241105154919133](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20241105154919133.png)

![image-20241105154733524](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20241105154733524.png)



**以太网ARP请求/应答报文详解**  

![image-20241105153150509](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20241105153150509.png)

❑硬件类型字段定义物理地址的类型，它的值为1表示MAC地址。❑协议类型字段表示要映射的协议地址类型，它的值为0x800，表示IP地址。
❑硬件地址长度字段和协议地址长度字段，顾名思义，其单位是字节。对MAC地址来说，其长度为6；对IP（v4）地址来说，其长度为4。
❑操作字段指出4种操作类型：ARP请求（值为1）、ARP应答（值为2）、RARP请求（值为3）和RARP应答（值为4）。
❑最后4个字段指定通信双方的以太网地址和IP地址。发送端填充除目的端以太网地址外的其他3个字段，以构建ARP请求并发送之。接收端发现该请求的目的端IP地址是自己，就把自己的以太网地址填进去，然后交换两个目的端地址和两个发送端地址，以构建ARP应答并返回之（当然，如前所述，操作字段需要设置为2）。

​	由图1-9可知，ARP请求/应答报文的长度为28字节。如果再加上以太网帧头部和尾部的18字节（见图1-6），则一个携带ARP请求/应答报文的以太网帧长度为46字节。不过有的实现要求以太网帧数据部分长度至少为46字节（见图1-4），此时ARP请求/应答报文将增加一些填充字节，以满足这个要求。在这种情况下，一个携带ARP请求/应答报文的以太网帧长度为64字节。  

**ARP高速缓存的查看和修改**  

![image-20241105153907517](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20241105153907517.png)

**使用tcpdump观察ARP通信过程**

​	为了清楚地了解ARP的运作过程，我们从ernest-laptop上执行telnet命令登录Kongming20的echo服务（已经开启），并用tcpdump（详见第17章）抓取这个过程中两台测试机器之间交换的以太网帧。具体的操作过程如下：  

![image-20241105163441348](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20241105163441348.png)

​	在执行telnet命令之前，应先清除ARP缓存中与Kongming20对应的项，否则ARP通信不被执行，我们也就无法抓取到期望的以太网帧。当执行telnet命令并在两台通信主机之间建立TCP连接后（telnet输出“Connected to 192.168.1.109”），输入Ctrl+]以调出telnet程序的命令提示符，然后在telnet命令提示符后输入quit，退出telnet客户端程序（因为ARP通信在TCP连接建立之前就已经完成，故我们不关心后续内容）。tcpdump抓取到的众多数据包中，只有最靠前的两个和ARP通信有关系，现在将它们列出（数据包前面的编号是笔者加入的，后同）：

![image-20241105163502740](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20241105163502740.png)

​	由tcpdump抓取的数据包本质上是以太网帧，我们通过该命令的众多选项来控制帧的过滤（比如用dst和src指定通信的目的端IP地址和源端IP地址）和显示（比如用-e选项开启以太网帧头部信息的显示）。

​	第一个数据包中，ARP通信的源端的物理地址是00:16:d3:5c:b9:e3（ernest-laptop），目的端的物理地址是ff:ff:ff:ff:ff:ff，这是以太网的广播地址，用以表示整个LAN。该LAN上的所有机器都会收到并处理这样的帧。数值0x806是以太网帧头部的类型字段的值，它表示分用的目标是ARP模块。该以太网帧的长度为42字节（实际上是46字节，tcpdump未统计以太网帧尾部4字节的CRC字段），其中数据部分长度为28字节。“Request”表示这是一个ARP请求，“who-has 192.168.1.109 tell 192.168.1.108”则表示是ernest-laptop要查询Kongming20的IP地址。

​	第二个数据包中，ARP通信的源端的物理地址是08:00:27:53:10:67（Kongming20），目的端的物理地址是00:16:d3:5c:b9:e3（ernest-laptop）。“Reply”表示这是一个ARP应答，“192.168.1.109 is-at 08:00:27:53:10:67”则表示目标机器Kongming20报告其物理地址。该以太网帧的长度为60字节（实际上是64字节），可见它使用了填充字节来满足最小帧长度。

![](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20241105163200520.png)

关于该图，需要说明三点：
- 我们将两次传输的以太网帧按照图1-6所描述的以太网帧封装格式绘制在图的下半部分。
- ARP请求和应答是从以太网驱动程序发出的，而并非像图中描述的那样从ARP模块直接发送到以太网上，所以我们将它们用虚线表示，这主要是为了体现携带ARP数据的以太网帧和其他以太网帧（比如携带IP数据报的以太网帧）的区别。
- 路由器也将接收到以太网帧1，因为该帧是一个广播帧。不过很显然，路由器并没有回应其中的ARP请求，正如前文讨论的那样。

##### DNS工作原理

我们通常使用机器的域名来访问这台机器，而不直接使用其IP地址，比如访问因特网上的各种网站。那么如何将机器的域名转换成IP地址呢？这就需要使用域名查询服务。域名查询服务有很多种实现方式，比如NIS（Network Information Service，网络信息服务）、DNS和本地静态文件等。本节主要讨论DNS。  

DNS是一套分布式的域名服务系统。每个DNS服务器上都存放着大量的机器名和IP地址的映射，并且是动态更新的。众多网络客户端程序都使用DNS协议来向DNS服务器查询目标主机的IP地址 。

**linux下访问DNS服务**

![image-20241105165406313](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20241105165406313.png)

host命令的输出告诉我们，机器名www.baidu.com是www.a.shifen.com.的别名，并且该机器名对应两个IP地址。host命令使用DNS协议和DNS服务器通信，其-t选项告诉DNS协议使用哪种查询类型。我们这里使用的是A类型，即通过机器的域名获得其IP地址（但实际上返回的资源记录中还包含机器的别名）。关于host命令的详细使用方法，请参考其man手册。

**使用tcpdump观察DNS通信过程**

![](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20241105165709556.png)

​	这两个数据包开始的“IP”指出，它们后面的内容描述的是IP数据报。tcpdump以“IP地址.端口号”的形式来描述通信的某一端；以“＞”表示数据传输的方向，“＞”前面是源端，后面是目的端。可见，第一个数据包是测试机器ernest-laptop（IP地址是192.168.1.108）向其首选DNS服务器（IP地址是219.239.26.42）发送的DNS查询报文（目标端口53是DNS服务使用的端口，这一点我们在前面介绍过），第二个数据包是服务器反馈的DNS应答报文。

​	第一个数据包中，数值57428是DNS查询报文的标识值，因此该值也出现在DNS应答报文中。“+”表示启用递归查询标志。“A?”表示使用A类型的查询方式。“www.baidu.com”则是DNS查询问题中的查询名。括号中的数值31是DNS查询报文的长度（以字节为单位）。

​	第二个数据包中，“3/4/4”表示该报文中包含3个应答资源记录、4个授权资源记录和4个额外信息记录。“CNAME www.a.shifen.com.，A 119.75.218.77，A 119.75.217.56”则表示3个应答资源记录的内容。其中CNAME表示紧随其后的记录是机器的别名，A表示紧随其后的记录是IP地址。该应答报文的长度为226字节。  

##### socket和TCP/IP协议族的关系  

​	1.数据链路层、网络层、传输层协议是在内核中实现的。因此操作系统需要实现一组系统调用，使得应用程序能够访问这些协议提供的服务。实现这组系统调用的API（Application Programming Interface，应用程序编程接口）主要有两套：socket和XTI。XTI现在基本不再使用  。
​	2.由socket定义的这一组API提供如下两点功能：一是将应用程序数据从用户缓冲区中复制到TCP/UDP内核发送缓冲区，以交付内核来发送数据（比如图1-5所示的send函数），或者是从内核TCP/UDP接收缓冲区中复制数据到用户缓冲区，以读取数据；二是应用程序可以通过它们来修改内核中各层协议的某些头部信息或其他数据结构，从而精细地控制底层通信的行为。比如可以通过setsockopt函数来设置IP数据报在网络上的存活时间。  
​	3.socket是一套通用网络编程接口，它不但可以访问内核中TCP/IP协议栈，而且可以访问其他网络协议栈（比如X.25协议栈、UNIX本地域协议栈等）。  

#### 第二章 IP协议详解

#### 第三章 TCP协议详解

#### 第四章 TCP/IP通信案例访问Internet



## 小林coding-计算机网络

### IP篇

子网掩码192.168.0.0/24 24是子网掩码的尾数网络部分占前24位 主机部分占后八位

![image-20241114211924698](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20241114211924698.png)

![image-20241114212515697](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20241114212515697.png)

CIDR无分类地址

![image-20241114213010327](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20241114213010327.png)

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





# 操作系统 

## 口袋操作系统

### 简介

操作系统位于应用程序和底层硬件之间，向上，为应用程序提供了虚拟化的底层资源，使得应用程序可以不考虑计算机在硬件层面的复杂性，有时也称操作系统为虚拟机；向下，操作系统又提供了针对硬件层面的资源管理，有时也被称为资源管理器

标准库和应用程序并不是以函数调用的方式使用系统调用，操作系统提供的系统调用，会被统一注册在相应的系统调用表这个内核结构中，上层代码在使用时，则需要以便宜位置的方式指定表中的某个单元格

![image-20240913161323270](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240913161323270.png)



**CPU虚拟化**： 每一个进程都是由独立的CPU进行处理的，任何一个进程的运行状态不会影响到其他进程

![image-20240913161902641](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240913161902641.png)

&符号可以将标记的进程置于后台运行

![image-20240913161726088](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240913161726088.png)

**内存虚拟化**：每个进程享有完全独立的内存资源。实际在操作系统内部，每个进程所享有的并不是全部的内存资源，而是自己独立的虚拟地址空间

![image-20240913162041757](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240913162041757.png)

**持久化** 

![image-20240913163622841](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240913163622841.png)

**并发** 应用程序层面最直接的体现时多线程，可以将进程中的任务进行更细致的划分，更好的利用多核CPU带来的任务并行性

![image-20240913163833324](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240913163833324.png)

操作系统的要求

![image-20240913164231510](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240913164231510.png)

![image-20240913164410265](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240913164410265.png)



### 进程

**进程的主要执行资源分为三部分：寄存器数据，内存数据，IO设置**

调度策略与上下文切换

![image-20240917105310922](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240917105310922.png)

上上下文切换：软件切换与硬件切换

![image-20240917105711344](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240917105711344.png)

调度策略

![image-20240917105809245](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240917105809245.png)

  虚拟地址空间：外面再调试C++打印的内存地址都是虚拟内存空间中的虚拟地址，而不是武理内存地址，这种针对进程的独立虚拟内存空间便是操作系统内存虚拟化的核心

![image-20240917110741484](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240917110741484.png)

 虚拟内存空间程序解析，栈通常是由高地址向低地址生长，所谓的栈顶实际上是栈空间再低地址方向的结束位置

![image-20240917110851311](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240917110851311.png)

 ![image-20240917111136949](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240917111136949.png)

IO设置

![image-20240917111310265](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240917111310265.png)

状态转移模型：在进程调度的整个生命周期中，进程可能会处于不同的状态，可以被最简划分为三中国：就绪/运行/阻塞，首先进程可能处于就绪状态，处于该状态的进程还没有被分配CPU时间片，因此还没有被执行，随着进程被调度，它被赋予相应的CPU时间片并开始运行，此时进程处于运行状态， 但在某一刻，进程执行了一些同步IO操作，比如读取文件，同步意味着读取文件在这个IO操作完成之前，进程的其他代码都无法被执行，未来更好的利用CPU，操作系统通常会将这类进程暂时设置为阻塞状态，处于阻塞状态的进程无法被直接执行，就直到某一刻，进程执行的同步IO操作结束了，操作系统再次将进程设置为就绪状态，并在合适的时间为其分配CPU时间片，当然处于运行状态的进程不会被一直运行，当手上的CPU时间片被使用完毕之后，他又会被置为就绪状态，等待操作系统的下一次调度

![image-20240917111534506](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240917111534506.png)

xv6操作系统 进程的内部结构

![image-20240917111705547](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240917111705547.png)

### 进程接口

类Unix系统中进程控制的经典模型

 ```c
 pid_t fork(void); //创建新进程 fork函数创建当前进程的拷贝作为子进程，子进程与父进程拥有一样的执行资源，拥有完全相同代码的Text端，并且正在同一位置执行，因此fork函数执行完毕之后，父子进程会一同从该函数返回
 pid_t wait(int* stat_loc);//等待子进程退出
 int execvp(const char *file, cha *const argv[]);
 //替换当前进程镜像。即进程的一系列执行资源包括寄存器资源，内存资源以及IO配置信息，会将当前进程的状态转换为可执行文件对应进程的状态
 ```

`execvp`可用于我们实现一些特殊点功能，比如输出重定向、管道

```c
int execvp(const char *file, char *const argv[]);
```

**参数**

- **file**：要执行的程序的名称。
- **argv[]**：参数列表，必须是以 `NULL` 结尾的指针数组。这使得 `execvp` 能够知道哪些参数传递给了新程序。

**功能**

`execvp` 会替换当前进程的映像，用新的程序来替换它。这意味着当前进程的代码、数据和堆栈都会被新程序的相应部分替换。**`execvp` 会搜索环境变量 `PATH` 指定的目录，以找到名为 `file` 的可执行文件。**

**使用场景**

`execvp` 常用在脚本或程序中需要调用外部程序时。例如，你可能想从你的 C 程序中调用文本编辑器或图像处理工具。

![image-20240917145752135](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240917145752135.png)

简易版shell

![image-20240917150118079](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240917150118079.png)

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <errno.h>

int main(int argc, const char* argv[]) {
  printf("Welcome to my shell!\n");
  while (1) {
    printf("# ");
    char *line = NULL;
    size_t size;
    size_t line_len = getline(&line, &size, stdin);
    if (line_len > 1) {
      // Remove line break character.
      *(line + line_len - 1) = '\0';  
      char* args[2];
      args[0] = line;
      args[1] = NULL;
      // Run command in a separate process.
      int ret_val = fork();
      if (ret_val < 0) {
        fprintf(stderr, "Fork failed!\n");
      } else if (ret_val == 0) {
        // Child process goes this way.
        int ret_code = execvp(args[0], args);
        if (ret_code == -1) {
          printf("Execution failed: %s.\n", strerror(errno));
        }
      } else {
        // Parent process goes this way.
        int wstatus;
        int terminated_pid = wait(&wstatus);
        if (terminated_pid == -1) {
          fprintf(stderr, "Wait failed!\n");
        }
        free(line);
      }
    }
  }
  return 0;
}
```

### 有限直接执行LDE模型

操作系统的CPU虚拟化，主要是通过分时控制的机制来实现的，分是看着主要通过两部分实现，调度策略（控制进程何时被切换）和上下文切换机制（暂停当前进程，并允运行另一个进程）

![image-20240917163130857](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240917163130857.png)

 用户态/内核台切换 系统调用

![image-20240917163921989](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240917163921989.png)

内核栈 用于完整表示原有进程的运行状态的寄存器的值，为了将执行流转移至用户进程，操作系统会在内核栈中安特定布局存放用于恢复进程的所有数据内容。恢复时将从内核栈拷贝至CPU物理寄存器

`Trap Tbale` `System Call`

![image-20240917164516686](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240917164516686.png)

![image-20240917164727831](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240917164727831.png)

![image-20240917165132853](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240917165132853.png)

![image-20240917165307545](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240917165307545.png)

非协作式进程切换 trap实际上就是一种由软件触发的中断 

每个进程在其基础南横控制块PCB中，通常存放两部分信息，第一部分为当操作系统调度该进程时，需要使用的内核栈地址，这**个地址指向的内存中存放着由内核想要运行这个进程时需要恢复的寄存器的值**，这些值将在操作系统以及return-from-trap指令的控制下，从内核栈拷贝至CPU的物理寄存器，第二部分为与该进程相关的上下文寄存器的值，不同于内核栈中保存的寄存器中的值，**上下文寄存器是指内核代码在调度进程时所需要的相关寄存器的值**，这些寄存器的值直接影响目标进程能否被顺利调度，当进程被顺利调度之后，内核栈中的寄存器中的值则可用于恢复进程的运行状态

![image-20240917170208675](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240917170208675.png)

![image-20240917170311873](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240917170311873.png)

![image-20240917170410433](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240917170410433.png)

