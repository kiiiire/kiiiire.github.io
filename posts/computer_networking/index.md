# Computer Networking


《计算机网络-自顶向下方法》笔记

 计算机网络课堂笔记，参考用书《计算机网络-自顶向下方法》。&lt;!--more--&gt;



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

# 应用层（Application Layer）

## 网络应用原理

- 可以在不同类型主机运行；
- 可以在不同终端间相互通讯；
- 在编写网络应用的过程中，不需要考虑网络核心设备，网络核心不会允许应用；
- 端系统上的应用可以快速开发，而且易于传播。

### 网络应用结构

#### 客户机-服务器体系结构（Client-server architecture）

 例如：HTTP、IMAP、FTP

**服务器**：

- 永远在线；
- IP地址恒定；
- 服务器往往在数据中心，通过多台服务器进行扩展。

**客户机**：

- 可以和服务器进行通信；
- 可能间断性连接网络；
- 可能是动态的IP地址；
- 客户机之间不会直接通信；

#### 点对点体系结构（Peer-peer architecture）

- 没有一个一直在线的服务器；
- 任意端系统之间直接进行通信；
- 每个点（peer）向其他的点请求服务，同时作为回报也会提供相应的服务；

优点：**自扩展性**（self-scalability）：新的点都会提供服务容量和负荷。

缺点：每个点都是间断性连接，而且IP地址会改变。

例子：P2P的文件分享。

### 进程通信（Processes Communicating）

#### 客户机和服务器进程

 **进程**（Processe）：一台主机上运行的程序。

- 在同一台主机上，两个进程通过**进程间通信**（inter-process communication）来进行通信。进程间通信由操作系统定义；

- 不同主机之间，进程通信同过报文交换，

  比如并行计算中的MP和MPI，MP（Multi Processing）只能用于同一台主机间的通信，MPI（Message Processing Interface）主要用于不同主机之间的通信，也适用于同一台主机。

 **客户机进程**（client process）：发起通信的进程。

 **服务器进程**（server process）：等待连接的进程。

P2P应用也存在客户机进程和服务器进程。

#### 套接字（Sockets）

 进程之间通过socket来接收/发送消息。

socket在进程通信中的作用相当于一个信封：

- 通信的信息需要装进socket
- 应用层下的各层作为基础设施，将信传到另一个进程
- 两个进程间的通讯会有两个socket

![socket.png](https://raw.githubusercontent.com/GY23333/imgs/master/socket.png)

#### 进程寻址（Addressing Processes）

 如果两个主机之间的进程进行通信，发送端不仅要知道接收端的IP地址还需要知道进程相应的端口号。

- **IP地址**：IPv4中32位IP，负责找到接收端主机。

- **端口号**（port number）：每台主机都可能运行着多个进程，每个进程对应一个端口号。

  比如，HTTP服务端口号80、邮件服务端口号25

### 应用层协议（Application-Layer Protocols）

 网络应用的开发必须遵守网络协议。

#### 应用层协议的分类

**公开网络协议**

- 定义在RFC中
- 统一标准，易于相互操作
- 例如：HTTP、SMTP

**专用网络协议**

- 一些非公开的网络协议
- 例如：Skype

#### 应用层协议内容

应用层协议定义了

- 消息交换的类型：比如请求、响应
- 消息的语法：消息中有哪些字段以及这些字段如何定义
- 消息的语义：消息字段内容的含义
- 规则：进程什么时候、如何发送/接收消息

#### app对传输服务的需求

- **数据完整性**（data integrity）

   一些app需要100%可靠的文件传输，比如文件传输；有一些运行一部分的丢失，比如语音。

- **时效性**（timing）

   一些app要求较少的延时，比如对话直播。

- **吞吐率**（throughput）

   吞吐率，即**最小带宽**，一些app存在一个吞吐率下限才能正常使用，比如视频音频等多媒体；有些app运行弹性的吞吐率，比如邮件传输，吞吐率小可以慢慢传过去。

![appsRequirements.png](https://raw.githubusercontent.com/GY23333/imgs/master/Network_appsRequirements.png)

### 网络传输协议服务

 **传输层**提供为应用层提供的两种传输服务。

#### TCP

 传输控制协议（TCP）

- **面向连接**（connection-oriented）：需要客户端和服务器之间能够建立连接
- **可靠传输**（reliable transport）：数据完整性高
- **流量控制**（flow control）：发送方不能发送太多数据导致接收方过载
- **阻塞控制**（congestion control）：不能有太多个主机同时发送导致网络过载
- 不提供的服务：时效性、最小带宽（吞吐率）、安全性

#### UDP

 UDP是一种**不提供不必要服务**的轻量传输协议。优点是速度快、灵活性好。

- **不需要建立连接**
- **不可靠的数据传输**
- 不提供的服务：基本都不提供，不提供包括可靠传输、流量控制、阻塞控制、时效性、最小带宽（吞吐率）、安全性

#### 不同app选择的网络传输协议

![img](https://raw.githubusercontent.com/GY23333/imgs/master/Network_appsProtocols.png)

## Web和HTTP

 World Wide Web 中的网页由超链接（hyperlink）连接。

- 页面由很多对象（object）组成，对象存储在服务器中；
- 对象有多种类型，可以是HTML文件，JPEG图片，动态脚本等等；
- 网页以HTML文件为基础，包括了许多参考对象，每个对象都可以通过URL来寻址。例如一张图片的URL为`www.someshcool.edu/someDept/pic.gif`，其中`www.someshcool.edu`为主机名，`/someDept/pic.gif`为路径名。

### HTTP概述

 超文本传输协议（hypertext transfer protocol, HTTP）

- 网页的**应用层**协议
- 基于**客户机-服务器体系结构**
  - 客户机：负责请求、接收和显示Web对象
  - 服务器：Web服务器负责发送对象，响应客户机请求

- HTTP的传输层使用

  TCP

  1. 客户机发起TCP连接（创建socket，**端口号80**）
  2. 服务器接收TCP连接
  3. 在浏览器和网页服务器之间进行HTTP信息的交换
  4. TCP连接可以断开

- HTTP是无状态的
  ​   服务器不会保留之前客户机发的请求信息。
  ​   协议要维持状态是很复杂的：保留之前的历史记录很消耗资源；如果客户机或着服务器有死机，它们的状态会不一致，还需要重新同步，这很麻烦。

- HTTP消息类型：请求（request）与响应（response）

### HTTP连接类型

#### 非持久性连接（Non-persistent HTTP）

##### 非持久性HTTP步骤

1. TCP连接开启

2. 通过这个TCP连接最多传输一个对象

3. TCP连接关闭

   如果要加载多个对象时，需要多次非持久性HTTP连接。

![Non-persistent%20HTTP(1).png](https://raw.githubusercontent.com/GY23333/imgs/master/Non-persistent%20HTTP(1).png)
![Non-persistent%20HTTP(2).png](https://raw.githubusercontent.com/GY23333/imgs/master/Non-persistent%20HTTP(2).png)

##### 非持久性HTTP响应时间

**RTT**：往返时间（Round Trip Time）,一个很小的数据包（处理文件的时间可忽略）从客户机传到服务器再传回来的时间。

**HTTP响应时间**（一个对象）：

- 1个RTT：建立TCP连接的时间

- 1个RTT：HTTP请求以及收到HTTP响应的前几个字符的时间

- 对象/文件传输的时间

  对一个对象来说，非持久性HTTP响应时间为

  

  Nonpersistent HTTP response time=2RTT&#43;file transmission timeNonpersistent HTTP response time=2RTT&#43;file transmission time

![Non-persistent response time.png](https://raw.githubusercontent.com/GY23333/imgs/master/NetWork%20Non-persistent%20HTTP%20response%20time.png)

例题：如果一个网页包含1个HTML和10个对象，则非持久性HTTP响应需要多少时间？



2RTT×(1&#43;10)&#43;total file transmission time2RTT×(1&#43;10)&#43;total file transmission time

##### 非持久性HTTP的问题

- 每传输一个对象都需要耗费 2RTT2RTT
- 每建立一个TCP连接都会对操作系统（OS）产生负荷
- 并行抓取：浏览器常常开多个并行的TCP连接去抓取对象

例题：一个网页包含1个HTML和10张图片，共有5个并行的TCP连接，则非持久性HTTP响应需要多少个RTT？
  首先传输HTML需要 2RTT2RTT 的时间，5个并行的TCP连接传输10个对象需要2个 2RTT2RTT 的时间。



RTT in Response Time=2RTT&#43;2×2RTT=6RTTRTT in Response Time=2RTT&#43;2×2RTT=6RTT

#### 持久性连接（Persistent HTTP）

##### 持久性HTTP步骤

1. 开启TCP连接
2. 通过这一个TCP连接可以传多个对象
3. TCP连接关闭

##### 持久性HTTP特点（HTTP1.1）

- 服务器在发送响应后保持连接开启状态
- 后续这个客户机\服务器的HTTP消息都通过该开启的连接发送
- 两种发送对象方式：HTTP1.1采用流水的方式发送：一次性把对象全发了；另一种是客户机接收到一个对象后接着发下一个对象的请求
- 至少需要1个RTT发完所有对象

## DNS

   域名系统（Domain Name System，DNS）通过分布式的数据库来实现IP地址和域名的映射。

- **层级结构的域名服务器提供分布式的数据库**
- **应用层协议**：主机和域名服务器通过通信来实现IP地址和域名的转换

DNS作为一个**网络核心功能**，为什么要放在应用层？
  与网络结构设计理念有关，网络中主机很多映射很复杂，希望将复杂度留在端系统中，而不是在网络核心。

### DNS服务

- IP地址和域名的转换
- 主机的别名
- 邮件服务的别名
- 负荷分配：有些Web可能有多个服务器，即会有多个IP地址对应一个域名，可调整IP地址的顺序以分配负荷。

### DNS：分布式、层级的数据库

为什么要选用分布的DNS，而不采用集中式的DNS？

- 单点可能失效：一个故障就gg了
- 流量问题：所有客户机都访问一个域名服务器，会产生很大的流量
- 远程的集中式数据库：客户机要访问可能要花费很多时间，RTT会很大，造成大时延

  DNS采用三层结构

![DNS Server.png](https://raw.githubusercontent.com/GY23333/imgs/master/Network_DNS%20Server.png)

一个客户机得到`www.amazon.com`的IP地址的步骤：

1. 客户机先查询根域名服务器，得到顶级域名服务器`.com DNS server`的地址；
2. 客户机查询顶级域名服务器`.com DNS server`，得到权威域名服务器`amazon.com DNS server`的地址；
3. 客户机查询权威域名服务器`amazon.com DNS server`，得到`www.amazon.com`的IP地址

#### 根域名服务器（Root Name Server）

- 官方的服务器，是连接的最后的方法（如果知道下级域名服务器的地址就不需要再从头开始查询根域名服务器，不然会对根域名服务器产生很大的流量负荷）
- 对网络运行相当重要，离开根域名服务器网络无法正常工作
- 域名系统安全扩展（Domain Name System Security Extensions，DNSSEC） —— 对DNS提供给DNS客户端（解析器）的DNS数据来源进行认证，并验证不存在性和校验数据完整性验证。
- ICANN（互联网名称与数字地址分配机构，Internet Corporation for Assigned Names and Numbers）—— 管理根域名服务器的组织

世界上现在有13个根域名服务器，分布在世界各地。

#### 顶层域名服务器（Top-Level Domain Server, TLD）

- 各种类型的TLD，比如`.com`、`.org`、`net`、`.edu`等等，国家的TLD，比如`.cn`、`.uk`等等。
- Network Solutions：管理`.com`、`net`TLD的组织
- Educause：管理`.edu`的组织

#### 权威域名服务器（Authoritative Domain Server）

- 组织自己的DNS服务器，用来提供组织内部的域名到IP地址的映射
- 由组织自己或者服务提供商来维护

#### 本地域名服务器（Local DNS Name Server）

- 严格来说不属于层级结构
- 每个ISP都会有一个本地域名服务器，也叫做**默认域名服务器**（default name server）
- 当主机要进行DNS查询时，查询会被直接送到本地的DNS服务器。
- 作用：
  - 缓存：可以缓存最近收到的域名到IP地址的映射（缓存有时效，会过期）
  - 代理：可以作为代理，代替主机在层级结构中进行查询

### DNS查询方法

#### 迭代查询（iterated query）

  被联系到的服务器会将后一个服务器的名字反馈回来，即“我不认识这个域名，但是你可以去问那台服务器(=ﾟωﾟ)ﾉ”

  下面的迭代查询过程，利用本地域名服务器作为代理迭代查询域名对于的IP地址。

![iterated query.png](https://raw.githubusercontent.com/GY23333/imgs/master/Network_iterated-query.png)

#### 递归查询（recursive query）

  把域名解析的负担交给了联系到的域名服务器，这种方法对于高级的负担增加，所以一般采用迭代查询而不采用递归查询。

![recursive query.png](https://raw.githubusercontent.com/GY23333/imgs/master/Network_recursive-query.png)

### DNS缓存和更新

- 一旦域名服务器学习到了一个映射，它就会**缓存**这个映射。缓存往往在本地域名服务器里，这样可以减轻根域名服务器的压力。
- **缓存有效时间TTL**，过了有效时间该缓存就会被删除。
- **更新/通知机制**：由IETF制定的 RFC2136 标准。
    如果中途域名主机改变IP地址，整个网络可能都不知道真正的IP地址，直到TTL到时，所以需要更新/通知机制。

### DNS记录和消息

#### DNS记录

  DNS的分布式数据库里存储了**资源记录**（resource record, RR）

RR的格式：`(name, value, type, ttl)`

**type = A**

- name：主机名
- value：对应的IP地址

**type = NS**

- name：域（如`foo.com`）
- value：对应的权威域名服务器的主机名

**type = CNAME**

- name：别名
- value：对应的规范主机名
- 比如 `www.ibm.com` 是 `servereast.backup2.ibm.com` 的别名，这与负荷的分配有关，可能有多个服务器。

**type = MX**

- name：邮件服务器别名
- value：对应的规范主机名

#### DNS消息

  DNS有两种消息类型：查询（query）和回答（reply），两种消息格式相同。

![DNS message.png](https://raw.githubusercontent.com/GY23333/imgs/master/Network_DNS-message.png)

#### 向DNS数据库插入记录

比如要创建一个`networkutopia.com`的网站

- 先在

  ```
  .com
  ```

  的TLD提供商 Network Solution 注册

   

  ```
  networkutopia.com
  ```

  - 提供信息：域名、权威域名服务器的IP地址
  - 提供商会向`.com`TLD服务器插入 NS、A 的RR
    (`networkutopia.com`, `dns1.networkutopia.com`, NS)
    (`dns1.networkutopia.com`, `212.212.212.1`, A)

- 在自己的权威域名服务器上进行配置

  - 插入`www.networkutopia.com`的type A记录
  - 如果是邮件服务，插入`networkutopia.com`的type MX记录

### DNS查询工具

  在命令行用`nslookup`进行DNS查询。

#### nslookup直接查询

  查询一个域名的A记录，如果没指定dns-server，用系统默认的dns服务器。

```
nslookup domain [dns-server]
```

#### nslookup查询其他记录

```
nslookup -qt=type domain [dns-server]
```

type可以是以下这些类型：

| type  | 类型                                |
| :---- | :---------------------------------- |
| A     | 地址记录                            |
| AAAA  | 地址记录                            |
| AFSDB | Andrew文件系统数据库服务器记录      |
| ATMA  | ATM地址记录                         |
| CNAME | 别名记录                            |
| HINFO | 硬件配置记录，包括CPU、操作系统信息 |
| ISDN  | 域名对应的ISDN号码                  |
| MB    | 存放指定邮箱的服务器                |
| MG    | 邮件组记录                          |
| MINFO | 邮件组和邮箱的信息记录              |
| MR    | 改名的邮箱记录                      |
| MX    | 邮件服务器记录                      |
| NS    | 名字服务器记录                      |
| PTR   | 反向记录                            |
| RP    | 负责人记录                          |
| RT    | 路由穿透记录                        |
| SRV   | TCP服务器信息记录                   |
| TXT   | 域名对应的文本信息                  |
| X25   | 域名对应的X.25地址记录              |

## P2P

  点对点体系结构（Peer-peer architecture）

### P2P概述

- 没有一个一直在线的服务器；
- 任意端系统之间直接进行通信；
- 每个点（peer）向其他的点请求服务，同时作为回报也会提供相应的服务；

优点：**自扩展性**（self-scalability）：新的点都会提供服务容量和负荷。

缺点：每个点都是间断性连接，而且IP地址会改变。

例子：P2P的文件分享。

### 文件分发：客户机-服务器结构 vs P2P

  从一个服务器分发大小为F的文件到N个节点需要多少时间？（每个节点上传和下载的速率都是有限的，网络中有足够的带宽）

![file distribution.png](https://raw.githubusercontent.com/GY23333/imgs/master/Network_file-distribution.png)

如果选用**客户机-服务器**结构

- 服务器上传：需要上传这份文件 NN 次，上传速度为 usus，则需要的上传时间为 NF/usNF/us

- 客户机下载：每个客户机都需要下载文件，dmindmin 是客户机最小下载速度，则客户机下载的最大时间为 F/dminF/dmin

    客户机-服务器结构的分发时间

  

  Dc_s≥max{NF/us,F/dmin}Dc_s≥max{NF/us,F/dmin}

    此处的N导致耗费的时间随要下载的节点的数量线性增长，当要下载的节点数目大时，要耗费相当多的时间。

    不需要先上传完再下载，参考[第一章/网络核心/分组交换](https://gy23333.github.io/2020/03/16/《计算机网络-自顶向下方法》笔记/#分组交换（Packet-Switching）)，以分组为单位发送，可以忽略上传到下载的时间。

如果选用**P2P**结构

- 服务器上传：服务器至少要上传1次文件，上传时间为 F/usF/us

- 客户机下载：每个客户机都要下载文件，客户机最大下载时间为F/dminF/dmin

- 客户机上传：每个下载了文件的客户机都可以上传文件，此时总上传速率可以达到us&#43;∑nuius&#43;∑nui

    P2P结构的分发时间

  

  DP2P≥max{F/us,F/dmin,NF/(us&#43;∑nui)}DP2P≥max{F/us,F/dmin,NF/(us&#43;∑nui)}

客户机-服务器结构和P2P分发时间对比

![vsPic.png](https://raw.githubusercontent.com/GY23333/imgs/master/Network_vsPic.png)

### BitTorrent

- 文件分为大小为 256Kb的块（chunk）

- 每个节点负责上传和下载的文件块

- 追踪器（tracker）：追踪参加洪流的节点

- 洪流（torrent）：有一组节点相互交换文件块

- 新的节点想下载文件，先询问追踪器参加的节点，再从相近的节点处下载文件块

  ![P2P file.png](https://raw.githubusercontent.com/GY23333/imgs/master/Network_P2P-file.png)

- 节点加入洪流：

  - 本身没有文件块，但是随着时间的推移会从其他节点获取文件块
  - 需要在追踪器进行登记，并且一般连接临近的节点

- 下载时，节点会上传文件块到其他节点

- 节点可以更改交换文件块的节点

- 节点随时会上线和下线

- 一旦节点有了完整的文件，它可以离开或者留在洪流中

#### 请求文件块

- 在给定的时间，不同的节点拥有不同的文件块
- 一定周期，新的节点会问每个节点有哪些块
- 新节点会从其他节点处下载缺失的文件块
  **最稀缺优先**（rarest first）：如果有10个节点都有第1、2块，只有一个节点有第3块，则先下载第3块。

#### 发送文件块

  发送文件块遵守**一报还一报原则（tit-for-tat）**

- 节点会给目前给它发送文件块速率最高的四个节点发送文件块，其他节点就不发送了，每隔 10s 会选出新的top4

- 每隔 30s 会随机选择其他节点发送文件块，这样这个随机节点可能就会成为新的top4

  tit-for-tat原则：上传速率快的节点相应地得到高下载速率的回报。

## socket编程

  目标：建立客户机/服务器的应用中的通信运用socket

  socket：相当于应用进程和点对点传输协议之间的一扇门

  socket类型：对应TCP和UDP有两种socket

### UDP中的socket编程

UDP：客户机与服务器之间没有连接

- 发送数据前不需要握手

- 发送数据包附加IP地址&#43;端口号

- 接收方从数据包中提取处IP地址&#43;端口号

    UDP提供的是一种不可靠的数据流传输，传输过程中可能会丢包，接收的时候顺序也可能被打乱。

  ![UDP socket.png](https://raw.githubusercontent.com/GY23333/imgs/master/Network_UDP_socket.png)

  #### UDP中的socket编程示例

    这里jupyter notebook中进行编程，安装好jupyter notebook后，在命令行执行

  ```
  jupyter notebook
  ```

  即可启动jupyter notebook

  UDP服务器代码

  ```
  UDPServer.ipynb
  from socket import * #引入socket库
  
  serverPort = 12000 #服务器端口号
  serverSocket = socket(AF_INET, SOCK_DGRAM) #创建服务器套接字
  serverSocket.bind((&#39;&#39;, serverPort)) #给套接字绑定端口号
  print(&#39;The server is ready to receive.&#39;)
  
  while True: #服务器要一直在线等待，所以给一个死循环
      message, clientAddress = serverSocket.recvfrom(2048) #从服务器套接字中读取信息（发送的消息和客户机IP地址&#43;端口号）
      modifiedMessage = message.decode().upper() #对消息进行处理（此处是改大写）
      
      serverSocket.sendto(modifiedMessage.encode(), clientAddress) #将处理后的消息发回给客户机
  ```

  UDP客户机代码

  ```
  UDPClient.ipynb
  from socket import * #引入socket库
  
  serverName = gethostname() #由于没得服务器，服务器主机用本机来当
  serverPort = 12000 #服务器端口号
  clientSocket = socket(AF_INET, SOCK_DGRAM) #创建客户机套接字
  
  message = input(&#39;Input lowercase sentence:&#39;) #得到输入字符串
  clientSocket.sendto(message.encode(),(serverName, serverPort)) #发送数据到相应主机名&#43;端口号的服务器进程
  
  modifiedMessage, serverAddress = clientSocket.recvfrom(2048) #接收服务器发回的消息
  print(modifiedMessage.decode()) #显示接收的字符串
  
  clientSocket.close() #关闭客户机socket
  ```

    先运行UDPServer.ipynb，启动服务器运行。

  ![UDP socket.png](https://raw.githubusercontent.com/GY23333/imgs/master/Network_UDP_socket_server.png)

    先运行UDPClient.ipynb，进行客户机访问。

  ![UDP socket.png](https://raw.githubusercontent.com/GY23333/imgs/master/Network_UDP_socket_client.png)

### TCP中的socket编程

- 服务器的先行准备

  - 服务器必须先运行
  - 服务器需要创建socket来连接客户机

- 客户机连接服务器

  - 客户机需要创建自己的socket，明确服务器进程的IP地址和端口号
  - 客户机创建socket时，客户机和服务器之间需建立TCP连接

- 服务器接收客户机消息

  - 服务器需创建一个新的socket，为了服务器进程能够和客户机进行通信
    - 要运行服务器与多个客户机进行通信
    - 用源的端口号来区分不同的客户机

    TCP提供的是一种可靠的字节流（byte-stream）传输（pipe）。

  ![TCP socket.png](https://raw.githubusercontent.com/GY23333/imgs/master/Network_TCP_socket.png)

#### TCP中的socket编程示例

TCP服务器代码

```
TCPServer.ipynb
from socket import * #引入socket库

serverPort = 12000 #服务器端口号
serverSocket = socket(AF_INET, SOCK_STREAM) #创建服务器套接字（前台）
serverSocket.bind((&#39;&#39;, serverPort)) #给套接字绑定端口号
serverSocket.listen(1)
print(&#39;The server is ready to receive.&#39;)

while True: #服务器要一直在线等待，所以给一个死循环
    connectionSocket, addr = serverSocket.accept() #前台套接字接收到请求后，创建一个新的套接字（窗口）
    
    sentence = connectionSocket.recv(1024).decode() #窗口套接字读取信息
    capitalizedSentence = sentence.upper() #对消息进行处理（此处是改大写）
    
    connectionSocket.send(capitalizedSentence.encode()) #将处理后的信息发回给客户机
    
    connectionSocket.close() #关闭窗口套接字，前台套接字保持开放
```



TCP客户机代码

```
TCPClient.ipynb
from socket import * #引入socket库

serverName = gethostname() #由于没得服务器，服务器主机用本机来当
serverPort = 12000 #服务器端口号
clientSocket = socket(AF_INET, SOCK_STREAM) #创建客户机套接字(类型为字节流SOCK_STREAM)
clientSocket.connect((serverName, serverPort)) #TCP连接

sentence = input(&#39;Input lowercase sentence:&#39;) #得到输入字符串
clientSocket.send(sentence.encode()) #发送数据到服务器

modifiedSentence = clientSocket.recv(1024) #接收服务器发回的消息
print(&#39;From server:&#39;, modifiedSentence.decode()) #显示接收的字符串

clientSocket.close() #关闭客户机socket
```



  先运行TCPServer.ipynb，启动服务器运行。

![TCP socket.png](https://raw.githubusercontent.com/GY23333/imgs/master/Network_TCP_socket_server.png)

  先运行TCPClient.ipynb，进行客户机访问。

![TCP socket.png](https://raw.githubusercontent.com/GY23333/imgs/master/Network_TCP_socket_client.png)


---

> Author: Kire  
> URL: http://localhost:1313/posts/computer_networking/  

