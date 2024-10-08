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

