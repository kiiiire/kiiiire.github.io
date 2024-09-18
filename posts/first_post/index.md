# My First Post


# 《计算机网络-自顶向下方法》笔记

 计算机网络课堂笔记，参考用书《计算机网络-自顶向下方法》。



# 计算机网络和因特网

## 计算机网络

 起源是为了形成一个自主、自治、互联的计算机通信系统。

### 构成描述

 因特网也被称为“网中网”，我们所用的网络都是大网中的小网（套娃）。

- 接入网络的设备——网络边缘

  - **主机**（host） = **端系统**（end system）
  - 运行的联网app

- 通信链路

  （communication link）——接入网

  - 媒介：同轴电缆、铜线、光纤、无线电频谱
  - 传输速率：带宽 bps

- 分组交换机

  （packet switch）——网络核心

  - **路由器**（router）和 **链路层交换机**（link-layer switch）

- **协议**：控制发送、接收信息。比如，TCP, IP, HTTP等等。每个层次有每个层次的协议。
- 网络标准
  - 因特网工程任务组（**IETF**）： 制定网络标准的组织
  - 请求评论（**RFC**）：IETF的标准文档

![img](https://i.loli.net/2020/03/19/J1qlUrQx9LiTFY2.png)

### 服务描述

- 为应用程序提供服务的基础设施

  即用户使用应用。

- 应用程序编程接口

  即编写应用。

## 网络边缘（Network Edge）

 与因特网相接的计算机及其他设备位于因特网的边缘，称为**端系统**。

端系统 = 主机，可以被划为下面两种：

- 客户（client）
- 服务器（server）：比如有企业存储大量数据的大型**数据中心**（data centers）

### 接入网（Access Networks）

 网络边缘的端系统 通过 **接入网**（物理链路） 连接到 **边缘路由器**（端系统到任何其他远程端系统的路径上的第一台路由器）。

#### 家庭接入：DSL、电缆、FTTH、拨号和卫星

- **数字用户线**（digital subscriber line，**DSL**）

 利用**电话线路**接入网络。其中 **ADSL**是非对称的数字用户线，基本都用ADSL，因为一般下行的数据量都远大于上行的数据量，所以要设计成非平衡的链路。

 采用**独占**的**频分多路复用**来传输。因为利用的是原有的电话线路，所以需要将DSL传输的网络信号（上行、下行）和电话信号通过频分多路复用来区分开来。

| 频段   | 传输信号     |
| :----- | :----------- |
| 0~4K   | 电话语音线路 |
| 4K~50K | 上行信号     |
| 50K~1M | 下行信号     |

![截屏2020-03-20上午12.22.20.png](https://i.loli.net/2020/03/20/oCDhqtVGuRLjenm.png)

- **电缆因特网接入**（cable Internet access）

  利用**有线电视网**接入网络。结构上，通过粗的同轴电缆接入社区，再用细的同轴电缆接入每家每户。

  采用**共享**的**频分多路复用**来传输。

![img](https://i.loli.net/2020/03/20/w1BdRQcMi5KS7Iz.png)

- **混合光纤同轴电缆**（**HFC**）

 **同轴电缆**和**光纤节点**相连再接入边缘路由器。

 **不对称**的**竞争式协议**，最高可达到30Mbps的下行速率和2Mbps的上行速率。由于采用竞争式协议，在用户少时使用体验优于普通的电缆因特网接入，但是在用户多时容易造成卡顿。

![img](https://i.loli.net/2020/03/20/xsZF9RbLfSq82or.png)

#### 企业（家庭）接入：以太网和WiFi

- **以太网**：使用双绞铜线与一台以太网交换机相连，速率可达到100Mbps、1Gbps、10Gbps。
- **WiFi**：IEEE802.11技术无线LAN，范围在几十米内。

#### 广义无线接入：4G和5G

 详见第六章

### 物理媒体（Physical Media）

- 导引型媒体（guided media）：信号在固体媒体中传输，比如光缆、双绞铜线和同轴电缆。
- 非导引型媒体（unguided media）：电波在空气中传播，比如无线局域网或数字卫星频道。

## 网络核心（Network Core）

 网络核心：由端系统的分组交换机和链路构成的网状网络。下图标亮部分即使网络核心。

 一共有三种交换方式：报文交换（很少使用）、分组交换和电路交换

![img](https://i.loli.net/2020/03/20/dAzBna3i9lZU2uN.png)

### 分组交换（Packet Switching）

 端系统之间彼此传输报文，分组交换中，将长报文划分为分组，分组再通过通信链路和分组交换机（分为路由器和链路层交换机）传送。

1. **存储转发传输**（Store-and-Forward Transmission）

    分组交换和报文交换都采用了存储转发的传输形式。但分组交换的存储转发以分组为单位，即交换机接收到整个分组后才能输出该分组的数据；而报文交换的存储转发单位为报文，需要交换机接收到整个报文后才能输出。

   ![img](https://i.loli.net/2020/03/20/YZt3aBmjsS1dLIi.png)

    传输相同大小的数据包，分组交换比报文交换更快。下面是分组交换传输3L大小的报文的时间流，报文分为3个大小为L的分组，根据分组交换原理，一共耗费了 4L/R 时间完成传输（即**存储转发时延**）。而如果使用报文传输同样的3L大小的报文则需要耗费 6L/R 时间。

   ![分组交换.jpg](https://i.loli.net/2020/03/20/zkxt9eo4fQqmLRV.jpg)

2. **排队时延**（Queuing Delay）和**分组丢失**（Packet Loss）

    分组交换机有有一个**输出缓存**（output buffer），分组可能会在分组交换机上排队等待输出，造成排队时延。

    分组交换机的缓存空间是有限的，所以在过于拥堵时会产生分组丢失（丢包）。

   ![img](https://i.loli.net/2020/03/20/iCtTZGq6BQuHzwa.png)

3. **转发表**（Forwarding Table）和**路由选择协议**（Routing Protocol）

   - **路由**：分组中包括IP地址；
   - **转发**：路由器中将目的地址映射为输出链路。

### 电路交换（Circuit Switching）

 **端到端连接**（end- to-end connection）：在发送数据之前，必须先在发送和接收两端建立端到端连接，并预留一部分带宽。而分组交换不预留，所以会造成排队和丢包。

![img](https://i.loli.net/2020/03/20/D91fBHdvxo24MFT.png)

#### 频分多路复用（FDM）

 链路中的每条连接专用一个频段。

![img](https://i.loli.net/2020/03/20/NubPYJA6WrcvsqI.png)

#### 时分多路复用（TDM）

 远距离传输会有衰减，所以考虑用**数字信号**进行传输。在时域上对信号进行**采样**，接收时再将采样信号恢复。

 TDM在时域上被划分为固定的**帧**（frame），每帧又被划分为固定数量的**时隙**（slot），链路中的每条连接专用一个时隙。

![img](https://i.loli.net/2020/03/21/yLN9bmafTdIqj6c.png)

### 分组交换 &amp; 电路交换

 分组交换的性能优于电路交换，适用于随机数据，可以满足更多用户。

 电路交换需要预留带宽，相当于固定了链路用户的数量。而分组交换不需要预留带宽，用户使用网络是有一定概率的，在一个时刻较多人使用的概率其实相对较低，所以一条链路可以给更多的用户使用。

 电路交换适用于特殊情况，比如要保障传输数据能力。

### 网络结构

 网络结构是网中之网，具有层次结构。

- **ISP**：ISP分为许多层级，比如**第一层ISP**（tier-1 ISP）、**区域ISP**（regional ISP）、**接入ISP**（access ISP）。端系统通过接入ISP与因特网相连，全球的ISP通过各个层级相连，形成了互联网的互联。
- 因特网交换点（Internet Exchange Point，**IXP**）：由第三方公司创建，IXP是一个汇合点，多个ISP在此处对等。

![img](https://i.loli.net/2020/03/21/NugS6r92GDaCqeJ.png)

## 协议（Protocol）

 协议定义了在两个或多个通信实体之间交换的报文格式和次序，以及报文发送和/或接收一条报文或其他时间所采取的动作。

 协议三大要素：

- 语法（Syntax）：每一段内容符合一定规则的格式，比如一个报文前8位是原地址，后八个是目的地址（只是举例，不要当真）之类。
- 语义（Semantics）：每一段内容需要代表某种意义，比如原地址部分的二进制到底是指哪个地址。
- 同步（Timing）：通信的过程，即每一段任务的执行顺序。

![img](https://i.loli.net/2020/03/22/RS7Lpk2YjGtCnKV.png)

## 协议层次（Protocol Layer）及其服务模型

### 五层因特网协议栈

 因特网协议栈由5个层次组成：物理层、链路层、网络层、传输层和应用层。因特网协议栈是一个理想模型。

 下层为上层提供服务。越下面的层，越靠近硬件；越上面的层，越靠近用户。

![截屏2020-03-22上午12.23.35.png](https://i.loli.net/2020/03/22/oYnb8OzPqg461Ux.png)

|            层次             | 功能                                                         |
| :-------------------------: | :----------------------------------------------------------- |
| 应用层（Application Layer） | 支持网络应用，应用协议仅仅是网络应用的一个组成部分，运行在不同主机上的进程则使用应用层协议进行通信。 |
|  传输层（Transport Layer）  | 负责为信源和信宿提供应用程序进程间的数据传输服务，这一层上主要定义了两个传输协议，传输控制协议即TCP和用户数据报协议UDP。 |
|   网络层（Network Layer）   | 负责将**数据报**独立地从信源发送到信宿，主要解决路由选择、拥塞控制和网络互联等问题。 |
|    链路层（Link Layer）     | 负责将IP数据报封装成合适在物理网络上传输的**帧**格式并传输，或将从物理网络接收到的帧解封，取出IP数据报交给网络层。 |
|  物理层（Physical Layer）   | 负责将**比特流**在结点间传输，即负责物理传输。该层的协议既与链路有关也与传输介质有关。 |

### OSI模型

 OSI模型由国际标准化组织（ISO）制定，实际并没有应用，只有理论。

 OSI模型由7层组成：应用层、表示层、会话层、传输层、网络层、数据链路层、物理层。

### 封装（Encapsulation）

 在发送端，

1. 应用层：将 **应用层报文**（application-layer message）M传送给传输层；

2. 传输层：接收报文M，附上传输层首部信息Ht（包括差错检测位信息等），构成 **传输层报文段**（transport-layer segment），将其传递给网络层；

3. 网络层：接收传输层报文段，附上网络层首部信息Hn（包括源和目的地址等），构成 **网络层数据段**（network-layer datagram），将其传递给网络层；

4. 链路层：接收网络层数据段，附上链路层首部信息Hl，构成 **链路层帧**（link-layer frame），将其传递给物理层；

5. 物理层：负责比特流物理传输。

   在接收端以反方向重构报文段。

![img](https://i.loli.net/2020/03/22/uBFK4rGifLvsHqE.png)

## 物理层概述

 在两个物理媒体间进行比特流传输，上层都是逻辑链接，只有物理层是实际的物理连接。

### 物理层基本目标

- 保证发送信号“0”和“1”的正确性以及发送和接收的一致性；
- 比特流传输的模式、速度、持续时间和信号失真；
- 接口设计：引脚数目、功能等等；
- 信号传输的程序：如何安排传输过程和事件次序。

### 物理层的基本特性

- **机械特性**：指明接口所用接线器的形状和尺寸、引线数目和排列、固定和锁定装置等；

- **电气特性**：指明传输模式、电压范围、编码、阻抗匹配、传输速率以及传输距离等等；

- **功能特性**：指明各条物理线路的功能，比如某条线上出现的某一电平的电压表示何种意义，

  物理接口信号线按功能分为四类：数据线、控制线、定时线和地线；

- **规程特性**：指明各物理线路工作规程和时序的关系，比如对于不同功能的各种可能事件的出现顺序，

  信号传输的模式：单工（仅单向通行）、半双工（双方通，但一个时刻仅一方通）、全双工（双方随时通）。

## 基本通信原理

### 基本概念

- 数据（data）：对于客观事实描述的物理符号，包括数字、文本、语言、图像等等；
- 信息（information）：数据的集合；
- 信号（signal）：数据传输中的表现形式，比如模拟信号、数字信号；
- 信道（channel）：往固定方向传输信息的媒介。

**模拟传输VS数字传输**

| 传输方式 | 优点             | 缺点           |
| :------- | :--------------- | :------------- |
| 模拟     | 对信道有高利用率 | 不抗噪         |
| 数字     | 信号不易失真     | 需要更宽的带宽 |

### 信道特性

- **码元**（Symbol）：承载信息量的基本信号单位。

   下面是马原分级数 N 与所需bit位数 n 的关系（N个离散的值需要n个bits）

  

  n=log2Nn=log2N

  ![img](https://i.loli.net/2020/03/22/wxHkuoMDth7Bqpm.jpg)

- **波特率**（Baud rate）：传输码元的速率。

- **比特率**（Bit rate）：传输比特的速率。

  波特率与比特率的关系

  

  bit rate(b/s)=baud rate∗nbit rate(b/s)=baud rate∗n

- **信道容量**（Channel capacity）：在一个信道中能够可靠地传送信息时可达速率的最小上界，单位 bps。

- **频带宽度**（Frequency bandwidth）：信道允许的信号频率范围，单位 Hz。

  

  Frequency bandwidth=maximum bandwidth−minimum bandwidthFrequency bandwidth=maximum bandwidth−minimum bandwidth

- **传输延迟**（Transmission delay）：包括发送到接受的处理事件、电信号的响应时间、中间介质的传输时间。

- **奈奎斯特定理**（Nyquist’s Law）

  1. 对于理想的低通信号，在一个频带宽度为 W 的信道，其码元的最大传输速率 = 2W Baud
  2. 对于理想的带通信号，在一个频带宽度为 W 的信道，其码元的最大传输速率 = W Baud

- **香农定理**（Shannon’s Formula）

  信道的信息传输速率C（单位：bps）

  

  C=Wlog2(1&#43;S/N)C=Wlog2(1&#43;S/N)

  - W - 带宽，单位 Hz

  - S - 信道的平均信号功率

  - N - 信道的高斯噪声功率

  - S/N - 信号功率与噪声功率之比（也可以叫信噪比）

    一般情况下，信噪比不用 S/NS/N 表示，而是 10log10S/N10log10S/N ，单位为dB。

### 模拟/数字信号传输

 模拟信道只能传输模拟信号，数字信道只能传输数字信号。

#### 调制解调器

 数字/模拟信号 经过 **调制** 变成 **模拟信号**，如此可在 模拟信道 上传输，再经过 **解调** 变成 数字/模拟信号。

![img](https://i.loli.net/2020/03/23/4mKvzSkIDigxhFX.png)

#### 编码解码器

 数字/模拟信号 经过 **编码** 变成 **数字信号**，如此可在 数字信道 上传输，再经过 **解码** 变成 数字/模拟信号。

![截屏2020-03-23上午1.37.30.png](https://i.loli.net/2020/03/23/OcMLw2JjiA9FsSP.png)

#### 调制（Modulation）：数字信号到模拟信号

 从数字信号到模拟信号的调制主要有三种方法：

- **调幅**（Amplitude Modulation，**AM**）

  把数字信号转化成不同幅度的正弦信号。

  ![img](https://i.loli.net/2020/03/23/tsgLBTpczPWhOHS.png)

- **调频**（Frequency Modulation，**FM**）

  把数字信号转化成不同频率的正弦信号。

  ![img](https://i.loli.net/2020/03/23/RJEuCxXSblUhyfV.png)

- **调相**（Phase Modulation，**PM**）

  把数字信号转化成不同相位的正弦信号。

  ![img](https://i.loli.net/2020/03/23/2NDtUwYbpmuHPVo.png)

#### 脉码调制（Pulse Code Modulation, PCM）

 模拟信号变成数字信号。

脉码调制步骤：

1. 采样（Sampling system）

   采样频率是信号频率的两倍，则可不失真地恢复原始信号。

2. 量化（Quantify）

   取整采样信号。

3. 编码（Coding）

   量化后的信号编码成二进制。

![img](https://i.loli.net/2020/03/23/ze59JcGyOojH2Dx.png)

#### 数字信号编码（Digital signal coding）

 数字信号到数字信号。

##### 不归零编码（Non-Return-To-Zero, NRZ）

 主要介绍两种不归零编码。

- Non-Return-To-Zero Level (**NRZ-L**) Coding

   高电平表示“1”，低电平表示“0”。

  ![img](https://i.loli.net/2020/03/23/XmcGZnlLOg92azH.png)

  NRZ-L的优点：简单好实现

  NRZ-L的缺点：

  - 难以分清二进制一位的开始和结束，所以必须要带**同步时钟**（外同步）来同步。可以把上图的虚线当作是同步时钟，当把虚线去掉是，相同电平的信号就不好判断开始和结束了。
  - 在传输全“1”或全“0”信号时，此时传输的只有直流分量（傅立叶分不出正弦或余弦分量），这样线路上会有比较大的噪声。

- 反向不归零编码（**NRZ-I**）

   遇“1”反向。只能解决NRZ-L的部分问题（全“1”问题，不再是直流）

  ![img](https://i.loli.net/2020/03/23/KQJyzFGPtanmhLk.jpg)

**外同步**（External synchronization）

 给系统一个同步时钟信号，设置一个周期的宽度。

 外同步有诸多不便，于是有了自同步的曼切斯特编码。

##### 曼切斯特编码（Manchester encoding）

 每个编码由两段组成，“1”：先高后低；“0”：先低后高（可以反过来定义）。

![img](https://i.loli.net/2020/03/23/CwpZhdnVO65m2DX.png)

曼切斯特编码的优点（解决了不归零编码的问题）：

- 自同步，不需要同步时钟；
- 直流分量为0。

曼切斯特编码的缺点：

- **基频增加**：基频是不归零编码的两倍，从而比特率变成了不归零编码的一半；

- **二义性**：组合情况有两种。

  ![img](https://i.loli.net/2020/03/23/3vlYkrz47V6qC1w.jpg)

##### 差分曼切斯特编码（Differential Manchester encoding）

 解决曼切斯特编码的二义性。

- “1”：自己的前半波与前一个编码的后半波相同；

- “0”：自己的前半波与前一个编码的后半波相反。

  有两种画法，初始为高、初始为低，两种画法结果对称。

![img](https://i.loli.net/2020/03/23/mdYhZEKHulpkjTW.png)

 差分曼切斯特编码的优点：

- 解决NRZ问题
- 解决二义性问题

 差分曼切斯特编码的缺点（同曼切斯特编码）：

- 基频翻倍
- 高频噪声增加

##### 块编码（Block encoding）

1. 将原信号每 m bits 分为一块；
2. 把 m bits 的每块映射成 n bits 的块（m &lt; n）；
3. 将 n bits 的块重新组合起来。

![img](https://i.loli.net/2020/03/23/5D78VTnaSXjMyfR.png)

### 多路复用（Multiplexing）

#### 频分多路复用（Frequency division multiplexing，FDM）

 见 [频分多路复用](https://gy23333.github.io/2020/03/16/《计算机网络-自顶向下方法》笔记/#频分多路复用（FDM）) 。

#### 时分多路复用（Time division multiplexing，TDM）

 见 [时分多路复用](https://gy23333.github.io/2020/03/16/《计算机网络-自顶向下方法》笔记/#时分多路复用（TDM）) 。

#### 波分多路复用（Wavelength division multiplexing，WDM）

 和FDM原理相同，但主要在光波中采用。

#### 码分多路复用（Code division multiplexing，CDM）

举例说明

 每台设备都给一个不同向量的码片，比如手机A的码片为A，手机B的码片为B。不同设备的码片正交，即



{A×B=0A×A=1{A×B=0A×A=1

 要向手机A发送数据a，向手机B发送数据b，则在信道中发送 a×A&#43;b×Ba×A&#43;b×B。

 在手机A接收数据时，将接收到的信号乘A。



(a×A&#43;b×B)×A=a×A×A&#43;b×B×A=a(a×A&#43;b×B)×A=a×A×A&#43;b×B×A=a

 同理手机B，如此两台设备同时利用一条信道传输，并成功分离各自的数据。

## 延时、丢包、吞吐量

### 延时（delay）

 现实中的计算机系统不是理想系统，事件是随机突发的，所以不可避免的存在延时。

![delay.png](https://raw.githubusercontent.com/GY23333/imgs/master/delay.png)

 数据包的延时由四个部分组成：



dnodal=dprop&#43;dqueue&#43;dtrans&#43;dpropdnodal=dprop&#43;dqueue&#43;dtrans&#43;dprop

- **处理时延**（Nodal Processing Delay，dprocdproc）

   路由器接收到分组对其进行处理的时间（比如差错检测），耗时很短，毫秒级。

- **排队时延**（Queuing Delay，dqueuedqueue）

   分组在链路上等待传输的时间，取决于先期到达的正在排队等待向链路传输的分组数量。

- **传输时延**（Transmission Delay，dtransdtrans）

   将分组的比特流传输到链路的时间（比如进行编码转换成查分曼切斯特编码的时间）。

   如分组长度 L ，传输速率 R bps，则其传输时延为：

  

  dtrans=L/Rdtrans=L/R

   所以传输时间取决于分组长度和传输速率。

- **传播时延**（Propagation Delay，$$）

   信号在媒体上传播的时间。

   如物理链路的长度 d，传播速度 s m/sec，则其传播时延为：

  

  dprop=d/sdprop=d/s

### 丢包（Packet Loss）

 流量强度（traffic intensity）代表了路由器上排队的拥堵率。



traffic intensity=(L×a)/Rtraffic intensity=(L×a)/R

 其中，L —— 分组长度（bits）；

 R —— 链路带宽（bps）；

 a —— 平均分组到达速率

- 流量强度接近0时，几乎没有分组到达，排队延时很小；
- 流量强度0～1时，平均排队长度越来越长，排队延时越来越长；
- 流量强度接近1时，存在到达率超过传输时间间隔，拥堵。

![traffic intensity.png](https://raw.githubusercontent.com/GY23333/imgs/master/traffic%20intensity.png)

 路由器的排队容量是有限的，当分组到达一个已满的队列时，路由器将丢弃该分组，产生丢包。

### 吞吐量（Throughput）

 从主机A到主机B传文件，B接收文件的速率为**瞬时吞吐量**（instantaneous throughput），单位bps；所有时间的平均速率为**平均吞吐量**（average throughput）。

 串联链路吞吐量取决于**瓶颈链路**（bottleneck link）。



Throughput=min{R1,R2,...,RN}Throughput=min{R1,R2,...,RN}

![Throughput.png](https://raw.githubusercontent.com/GY23333/imgs/master/Throughput.png)


---

> Author: Kire  
> URL: http://localhost:1313/posts/first_post/  

