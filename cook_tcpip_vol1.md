# TCP/IP 卷1

## IP地址的类别

### IP地址分类
<pre>
          7 bit        24 bit
          +-+-------+------------------------+
  A Type  |0|Net ID | Host ID                |
          +-+-------+------------------------+

                14 bit        16 bit
          +--+--------------+----------------+
  B Type  |10| Net ID       | Host ID        |
          +--+--------------+----------------+

                  21 bit              8 bit
          +---+---------------------+--------+
  C Type  |110| Net ID              |Host ID |
          +---+---------------------+--------+

                     28 bit
          +----+-----------------------------+
  D Type  |1110|   Multicast ID              |
          +----+-----------------------------+

                     27 bit
          +-----+----------------------------+
  E Type  |11110|  Left for later use        |
          +-----+----------------------------+
</pre>

类型|范围
:-:|:--
A|0.0.0.0 ~ 127.255.255.255
B|128.0.0.0 ~ 191.255.255.255
C|192.0.0.0 ~ 223.255.255.255
D|224.0.0.0 ~ 239.255.255.255
E|240.0.0.0 ~ 255.255.255.255 不明白为啥右边界不是247

- - - -

## TCP/IP协议簇的四个层次
顺序从底层到上层
1. 应用层
  >只负责自己应用的细节。 示例的应用程序: Telnet, FTP, SMTP(简单邮件传送协议), SNMP(简单网络管理协议)
2. 传输层
  >为应用层的程序提供端对端的通信。 示例的传输协议: TCP(传输控制协议), UDP(用户数据报协议)
3. 网络层
  >处理传输层的分组在网络中活动，例如分组的选路。 示例网络协议: IP协议(网际协议), ICMP协议(Internet互联网控制报文协议), IGMP协议(Internet组管理协议)
4. 链路层
  >设备驱动程序和网络接口卡。示例：以太网协议

- - - -

## 路由器
又称IP路由器(IP Router) 用途: 为不同类型的物理网络(链路层协议协议)提供链接: 以太网、令牌环网、点对点的链接和FDDI(光纤分布式数据接口)等等。所以路由器需要对不同链路层进行抽象，网络层就是这一个抽象(网络层的由来)。
<pre>
  +----------+                             FTP Protocol                                  +----------+
  |FTP Client|<------------------------------------------------------------------------->|FTP Server|
  +----------+                                                                           +----------+
       ^                                                                                      ^
       |                                                                                      |
       |                                                                                      |
       |                                                                                      |
       v                                                                                      v
  +---------+                              TCP Protoco                                   +----------+
  |   TCP   |<-------------------------------------------------------------------------->|   TCP    |
  +---------+                                                                            +----------+
       ^                                                                                      ^
       |                                                                                      |
       |                                                                                      |
       |                                       IP Router                                      |
       v                         +-------------------------------------+                      v
  +---------+     IP Protocol    |             +--------+              | IP Protocol     +----------+
  |    IP   |<-------------------+-----------> |   IP   | <------------+---------------->|    IP    |
  +---------+                    |             +--------+              |                 +----------+
       ^                         |               /    \                |                      ^
       |                         |              /      \               |                      |
       |                         |             /        \              |                      |
       |                         |            /          \             |                      |
       V                         |           /            \            |                      v
+--------------+  Ether Protocol | +------------+       +------------+ |Token Protocol +-------------+
|Ether Driver  |<----------------->|Ether Driver|       |Token Driver|<--------------->|Token Driver |
+--------------+                 | +------------+       +------------+ |               +-------------+
       |                         +--------|-------------------|--------+                      |
       V                                  v                   |                               |
   -----------------------------------------                  v                               v
            Ether Net                                  +-----------------------------------------+
                                                       |                Token Net                |
                                                       +-----------------------------------------+
</pre>
Ether: 以太网
Token: 令牌环

连接网络的另一个途径是网桥，网桥是在链路层对网络进行互连，而路由器则是在网络层对网络进行互。网桥使得多个局域网(LAN)组合在一起，对上层来说就好像是一个局域网。

- - - -

## 不可靠的IP层 与 可靠的TCP
TCP/IP协议族中，IP网络层提供的是不可靠的服务(只是尽可能快的把分组从源节点送到目的地点)；TCP在不可靠的IP层提供可靠的传输层，采用超时重传、发送和接收端到端的确认分组等机制保证可靠性！

- - - -

## ICMP 与 IGMP
- ICMP IP协议的附属协议。IP层用来与其他主机或路由器交换错误报文和其他重要信息。Ping和Traceroute工具就是使用ICMP
- IGMP Internet组管理协议。用来把UDP数据报多播到多个主机。

- - - -

## 封装与分用
TCP/IP 不同层协议数据报传递的过程
### 封装
数据报从应用层-\>传输层-\>网络层-\>链路层传递的过程。其实就是下一层协议把从上一层协议拿到的数据报加上自己头部或尾部(链路层会加上尾部), 这些额外的信息就是为了分用服务的。<br>

层次|数据报的名称|报头独特的意义
:--|:--|:--
传输层|段Segment(比如:TCP段)|指明应用程序(通过端口号)
网络层|IP数据报(IP datagram)|指明协议类型(不一定是传输层协议): TCP,UDP,ICMP,IGMP
链路层|帧Frame|指明Frame是哪种网络层协议:IP,ARP,RARP

### 分用
封装的逆过程。底层协议根据数据报头的找到各自的上层协议。

- - - -

## 最大传输单元MTU
以太网和802.3对数据帧的最大长度限制，IP数据报如果比链路层的MTU大，就要进行分片(fragmentation)。

### 路径MTU
两台主机进行通行时，如果处于同一个网络，那路径MTU就是该网络链路层的MTU，但如果处于不同的网络，那么通信路径所经过的网络的最小MTU就称为路径MTU。
- 路径MTU不一定是常数，取决于所选的路由
- 路径MTU不一定是对称，也是取决与路由选择

- - - -

## IP协议的不可靠与无连接
- 不可靠(unreliable)
> 不能保证IP数据报能成功地到达目的地
- 无连接(connectionless)
> IP不维护任何关于后续数据报的状态信息

## IP首部

下面数字单位是bit
<pre>
 0                                                                                                  31
 +------------+------------+------------------------+------------------------------------------------+-----
 |4 Version   |4 Head Size |8 Server Type (TOS)     |16 IP Datagram Total Size                       |  ^
 +------------+------------+------------------------+---------+--------------------------------------+  |
 |16 Identification                                 |3 Flag   |13 Slice Offset                       |  |
 +-------------------------+------------------------+---------+--------------------------------------+  |
 |8 time-to-live(TTL)      |8 Protocol              |16 Header Checksum                              | 20 Bytes
 +-------------------------+------------------------+------------------------------------------------+  |
 |32 Source IP Address                                                                               |  |
 +---------------------------------------------------------------------------------------------------+  |
 |32 Destination IP Address                                                                          |  v
 +---------------------------------------------------------------------------------------------------+-----
 .Options                                                                                            .
 +---------------------------------------------------------------------------------------------------+
 .                                                                                                   .
 |Datas                                                                                              |
 .                                                                                                   .
 +---------------------------------------------------------------------------------------------------+

注: 最高位在左边，记0bit；最低为在右边，计为31bit
    网络字节序: 4个字节的32bit以0~7 8~15 16~23 24~31的顺序传输，即是高位在低地址，地位高地址，称为Big-Endian字节序。
</pre>

### Version
IPv4 Version字段就是4

### Head Size
首部长度就是IP首部的大小(单位是32bit,4bytes), 4bit最大表示值是15, 15×4=60， 所以首部最长为60个字节。普通IP数据报(没有选项字段)的值为5,即20个字节大小。

### Service Type(TOS)
服务类型TOS，可以设置: 最小延迟，最大吞吐量，最高可靠性，最小费用。不过现在大多数的TCP/IP实现都不支持TOS

### IP Datagram total size
整个IP数据报的长度(单位:Bytes)，16bit最大表示值是65535,所以IP数据报最大是65535字节。

### Identification
标识字段唯一地标识主机发送的每一份数据报(通常每发送一份报文就会+1)

### Flag
标志字段

### TTL(time-to-live)
数据报可以经过的最多路由器数。TTL的初始值由源主机设置(通常为32或64),一旦经过一个处理的路由器就减1,当该字段为0时，数据报就会被丢弃，并发送ICMP报文通知源主机。

### Head Checksum
首部校验和根据IP首部计算的校验和码。

### Options
- 安全和处理限制
- 记录路径(记录下经过的路由的IP地址)
- 时间戳(记录下经过的路由的IP地址和时间)
- 宽松的源站选路(为数据报指定一系列必须经过的IP地址)
- 严格的源站选路(为数据报指定一系列只能经过的IP地址)

- - - -

## 特殊情况的IP地址

<table>
<tr>
<th colspan='3'>IP地址</th>
<th colspan='2'>可以为</th>
<th rowspan='2'>描述</th>
</tr>
<tr> <th>网络号</th> <th>子网号</th> <th>主机号</th> <th>源端</th> <th>目的端</th> </tr>
<tr> <td>0</td><td/><td>0</td> <td>OK</td> <td>不可能</td> <td>网络上的主机</td> </tr>
<tr> <td>0</td><td/><td>hostid</td><td>OK</td><td>不可能</td><td>网络上特定的主机</td> </tr>
<tr> <td>127</td><td/><td>任何值</td><td>OK</td><td>OK</td><td>环回地址</td> </tr>
<tr> <td>-1</td><td/><td>-1</td><td>不可能</td><td>OK</td><td>受限的广播(永远不会被转发)</td> </tr>
<tr> <td>netid</td><td/><td>-1</td><td>不可能</td><td>OK</td><td>以网络为目的向netid广播</td> </tr>
<tr> <td>netid</td><td>subnetid</td><td>-1</td><td>不可能</td><td>OK</td><td>以子网为目的向netid、subnetid广播</td> </tr>
<tr> <td>netid</td><td>-1</td><td>-1</td><td>不可能</td><td>OK</td><td>以所有子网为目的向netid广播</td> </tr>
</table>
-1表示全为0

- - - -

## ARP与RARP

<pre>
  +----------------+
  |32 bits IP Addr |
  +----------------+
        |   ^
        |   |
    ARP |   | RARP
        |   |
        v   |
  +----------------+
  |48 bits MAC Addr|
  +----------------+
</pre>

### ARP
主机A在以太网络里面，要向IP地址为a.b.c.d的主机或路由器发送数据报，就需要把a.b.c.d这个地址转成对应的MAC(物理硬件地址), 这就是ARP的功能。

ARP本来就是用来广播网络的，ARP发送一份称作ARP请求的以太数据帧给以太网上的每个主机，ARP请求帧里面包含目的主机的IP地址，意思“如果你是这个IP地址的拥有者，请回答你的硬件地址”

#### ARP分组格式解析

<pre>
                                                               +-->Hardware Address Length
                                                               |
                                                               |   +--->Protocol Address length
                                                               v   v
  +------------------+------------------+------+------+------+---+---+------+------------------+------------+------------------+------------+
  |Destination Addr  |Source Addr       |F Type|H Type|P Type|HAL|PAL|  op  |Src Ether Addr    |Src IP Addr |Dst Ether Addr    |Dst IP Addr |
  +------------------+------------------+------+------+------+---+---+------+------------------+------------+------------------+------------+
  |    6 Bytes          6 Bytes           2    |  2     2      1   1    2           6                4             6                4       |
  |                                            |                                                                                            |
  +<------------ Ether Frame Header ---------->+<----------------------------- 28 Bytes ARP Request/Reply --------------------------------->+

</pre>

字段解析

字段|含义
:--|:--
Destination Addr|以太网的源地址
Source Addr|以太网的目的地址，全为1的特殊地址为广播地址，电缆上的所有以太网接口都要接受广播的数据帧
F Type|帧类型，表示后面数据的类型。ARP请求/应答的帧类型是0x0806; RARP请求/应答的帧类型是0x8035
H Type|硬件类型表示硬件地址的类型，1 表示以太网地址
P Type|协议类型表示要映射的协议地址类型，0x0800 表示IP地址
Hardware Address Length|硬件地址长度，以太网地址长度为6个字节
Protocol Address Length|协议地址长度，IP地址长度为4个字节
op|操作类型。ARP请求(1), ARP应答(2), RARP请求(3), RARP应答(4)


#### ARP代理
如果ARP请求是从一个网络的主机发送到另外一个网络的主机，那么连接两个网络的路由器就可以回答该请求，这个过程就称作委托ARP或ARP代理(Proxy ARP)。通过命令 `arp -a` 如果发现连个IP地址对应同一个MAC，就是Proxy ARP。

#### 免费ARP
免费ARP(gratuitous ARP) 就是指主机发送ARP查找自己的IP地址
- 一个主机通过它来确定另一个主机是否设置了相同的IP地址;
- 如果发送免费ARP的主机正好修改了硬件地址，这个分组就可以使其他主机高速缓存中旧的硬件地址进行相应的更新;

- - - -

## RARP
RARP协议是许多无盘系统在引导时用来获取IP地址的，RARP的分组格式与ARP相同，帧类型也一样，指示op操作字段不同！

- - - -

## ICMP Internet控制报文协议
传递错误报文以及其他需要注意的信息，通常被IP层或更高协议(TCP或UDP)使用。虽然ICMP被认为是IP层的组成部分，但ICMP报文是在IP数据报里面传输的!
<pre>
 |<--------- IP Datagram -------------------------->+
 |                                                  |
 +----------------+---------------------------------+
 | IP Header      |  ICMP Datagram                  |
 +----------------+---------------------------------+
    20 Bytes
</pre>

### ICMP报文

#### ICMP的类型由类型字段和代码字段共同决定。ICMP报文分为两大类: 差错报文和查询报文！永远不会对差错报文响应另外一个差错报文！
<pre>

  0                7              15 16                              31
  +----------------+----------------+--------------------------------+
  | 8 Type         | 8 Code         | 16 checksum                    |
  +----------------+----------------+--------------------------------+
  |                                                                  |
  z                                                                  z
  . Diff Type & Code with Diff Contents                              .
  z                                                                  z
  |                                                                  |
  +------------------------------------------------------------------+
</pre>

#### 以下情形也不会产生差错报文(这些限制是为了防止广播风暴！)

- ICMP差错报文
- 目的地址是广播地址或多播地址的IP数据报
- 作为链路层广播的数据报
- 不是IP分片的第一片
- 源地址不是单个主机的数据报 即源地址不能为零地址、环回地址、广播地址或多播地址。

#### ICMP 差错报文
ICMP差错报文必须包括生成该差错报文的数据报的IP首部(可用于判断传输层协议)和IP首部后面的前8个字节(可用于判断端口号)

- - - -

## Ping
Ping程序就是通过发送ICMP回显请求到目的主机，通过ICMP回显应答报文来判断目的主机是否可达！

- - - -

## Traceroute
Traceroute可以找出源地址到目的地址之间经过的路由路径。

Traceroute程序就是使用ICMP报文和IP首部中的TTL字段。当路由器收到一份IP数据报，如果TTL字段是0或1, 则路由器不能转发该数据报(接受到这种数据报的目的主机可以将它交给应用程序), 路由器將该数据报丢弃，并给信源机发送一封ICMP“超时”信息，Traceroute正是根据这封ICMP信息的IP报文的信源地址是该路由器的IP地址！ 

Traceroute工作流程:
1. 发送1份TTL为1的数据报给目的主机，如果收到ICMP超时报文，就记下报文的源地址(路由节点)并递增TTL后继续发送数据报，直到没有收到ICMP超时报文;
2. 由于目的主机收到数据报之后不会再发送ICMP超时报文，所以Traceroute程序需要再发送1份UDP数据报给目的主机(选择一个不可能的UDP端口), 使目的主机产生"端口不可达"的ICMP报文, Traceroute就根据这个UDP请求报文的回复的ICMP报文是“超时”还是“端口不可达”来判断什么时候结束;

- - - -

## IP选路
IP层进行的选路是这一种选路机制，搜索路由表并决定向哪个网络接口发送分组。<br>
搜索路由表的步骤:
1. 搜索匹配的主机地址;
2. 搜索匹配的网络地址;
3. 搜索默认表项;

### 查看路由表 netstat -r 
<pre>
➜  ~ netstat -rn #-n选项以数字格式打印IP地址
Kernel IP routing table
Destination     Gateway         Genmask         Flags   MSS Window  irtt Iface
0.0.0.0         192.168.0.1     0.0.0.0         UG        0 0          0 wlp2s0
169.254.0.0     0.0.0.0         255.255.0.0     U         0 0          0 wlp2s0
192.168.0.0     0.0.0.0         255.255.255.0   U         0 0          0 wlp2s0
➜  ~ netstat -r
Kernel IP routing table
Destination     Gateway         Genmask         Flags   MSS Window  irtt Iface
default         192.168.0.1     0.0.0.0         UG        0 0          0 wlp2s0
link-local      *               255.255.0.0     U         0 0          0 wlp2s0
192.168.0.0     *               255.255.255.0   U         0 0          0 wlp2s0
</pre>

### Flags含义

标志|意义
:--|:--
U|该路由可以使用
G|该路由是到一个网关(路由器),如果没有设置此标志，则目的地是直接相连
H|该路由是一个主机，如果没有设置该标志，则目的地址是一个网络号
D|该路由是重定向报文创建的
M|该路由已被重定向报文修改

**标志G**区分了间接路由(有G标志)和直接路由，发往直接路由的分组不但具有目的端的IP地址，还具有其链接层路径。而发往间接路由的分组：IP地址指明的是最终目的地，但链路层地址指明的是网关(下一站路由)!<br>
**标志H**区分主机地址和网络地址，注意：搜索路由表是，主机地址必须与目的地址完全匹配，而网络地址只需匹配目的地值的网络号和子网号!<br>

- - - -

## ICMP重定向错误
当IP数据报发送到的路由器不是该数据报的下一站时(通过查询路由表), 该路由器除了转发该IP数据报之外，还发送ICMP重定向错误告诉IP数据报的源主机，告诉它更新路由表。

- - - -

## 动态选路协议
用于路由器间的通信。RIP(Routing Information Protocol)选路信息协议是最广为使用的选路协议。RIP报文包含在UDP数据包中。(RIP常用的UDP端口是520)

- - - -

## UDP用户数据报协议
UDP是一个简单的面向数据报的传输层协议。

UDP 封装
<pre>
 +<--------------------------- IP Datagram ---------------------------------->|
 |                                                                            |
 |                                        |<------- UDP Datagram ------------>| 
 +----------------------------------------+----------------+------------...---+
 |  IP Header                             |UDP Header      |  UDP Data        |
 +----------------------------------------+----------------+------------...---+
   20 Bytes                                 8 Bytes
</pre>

UDP 首部
<pre>
 0                              15 16                             31
 +--------------------------------+--------------------------------+ 
 |16 bit Source Port              |16 bit Destination Port         | 
 +--------------------------------+--------------------------------+ 
 |16 bit UDP length               |16 bit Checksum                 | 
 +--------------------------------+--------------------------------+ 
 |                                                                 |
 z                                                                 z
 z                            Data                                 z
 z                                                                 z
 |                                                                 |
 +-----------------------------------------------------------------+
</pre>

### UDP校验和
UDP校验和覆盖UDP头部和UDP数。与IP首部的校验和不同，后者只覆盖IP的首部。UDP的校验和是可选的，TCP的校验和是必须的！

- - - -

## IP分片
IP层接收到发送的IP数据报后，判断向本地哪个接口发送数据(选路)之后，查询该接口的MTU，IP把MTU与数据报长度进行比较，如果数据报长度比MTU大，则需要对IP数据报进行分片。


**几个要点**
- 分片可以发生在源端主机上，也可以发生在**中间路由器**上;
- IP数据报分片之后，只有到达目的地才进行重新组装;
- IP分片对传输层是透明的，IP层本身没有超时重传的机制，所以只丢失一片数据也要重新传整个IP数据报;
- 分片除了最后一片，其他每一片中数据部分必须是8字节的整数倍;
- 分片后，只有第一片有UDP首部，其余的分片没有;

**需要分片导致的ICMP不可达差错**<br>
当路由器收到一份需要分片的数据报时，而IP首部又设置了不分片(DF标志), 路由器就会发送一份包含该链路层MTU的ICMP差错报文。这个差错可以用做路径MTU发现机制！

**IP层的定时器**<br>
IP层接受端会在收到第一个数据报(第一个到达)的时候，启动一个定时器(正常可能为30s或60s),如果定时器超时而该数据报的所有数据报片未全部到达，那么將这些数据报片全部丢弃。如果不这么左，会引起接受端缓存爆掉！

**ICMP源站抑制差错(source quench)**<br>
发送UDP数据报是，当一个系统(路由器或主机)接受数据报的速度比其处理速度快时，可能产生这个差错。

- - - -

## 协议栈各层对收到帧的过滤过程

流程: 网卡查看由信道传送过来的帧，确认是否接受该帧，若接受就將它传送给设备驱动程序。网卡通常仅接受目的地址为网卡网络地址或广播地址的帧，除非支持混杂模式的接口(tcpdump使用这种模式)，网卡还会过滤掉帧检验出错的帧。设备驱动程序随后將数据帧传送给上一层。IP层会根据IP地址中的目的地址和源地址进行更多的过滤检验，如果正常，就继续传给上一层(TCP或UDP)。UDP或TCP层收到IP层传来的数据报，根据目的端口号，如果没有进程使用该端口号，就丢弃数据报并产生一个ICMP不可达报文。

<pre>                 
      deliver
         ^
         |
     +-------+
     |  UDP  |
     +-------+
         ^
         | deliver
         |
     +-------+
     |  IP   |----> discard
     +-------+
         ^                  
         | deliver          
         |                  
  +-------------+                
  |device driver|----> discard   
  +-------------+                
         ^
         |
         |
    +---------+
    |Net Card |----> discard 
    +---------+
</pre>

- - - -

## 广播与多播
广播是將数据报发送到网络中的所有主机(通常是本地相连的网络),而多播是將数据报发送到网络中的一个主机组。

### 广播
四种IP广播地址

#### 受限的广播

255.255.255.255 就是受限的广播地址,有以下特性：
- 用于主机配置过程中IP数据报的目的地址(因为此时主机可能不知道它所在网络的网络掩码，甚至自己的IP都不知道);
- 路由器不能转发目的地址为受限的广播地址; 

#### 指向网络的广播
主机号全为1的地址就是指向网络的广播地址。比如:A类的网络广播地址为 netid.255.255.255。路由器必须转发指向网络的广播，但也必须有一个不进行转发的选择。

#### 指向子网的广播
主机号全为0且有特定子网号的地址就是指向子网的广播地址。 例如：B类网络128.1的子网掩码为255.255.255.0时，128.1.2.255就是指向该子网的广播地址。

#### 指向所有子网的广播
指向所有子网的广播需要了解目的网络的子网掩码，才能与指向网络的广播地址区分开。例如：目的子网掩码为255.255.255.0, 那么IP地址128.1.255.255是一个指向所有子网的广播地址，然而如果网络没有划分子网，那就是指向网络的广播地址。


### 多播

多播组地址
<pre>
     +---+---+---+---+---------------------------------------------------------------------------+
 D   | 1 | 1 | 1 | 0 |                   Multicast ID (28bits)                                   |
     +---+---+---+---+---------------------------------------------------------------------------+
</pre>

能够接受发往一个特定多播组地址数据的主机集合成为主机组(host group)。一个主机组可跨越多个网络。主机组中成员可随时加入或离开，主机组对主机的数量没有限制。<br>
一些多播组地址被IANA确定为知名地址，例如: 224.0.0.1代表"该子网内的所有系统组", 224.0.0.2表示"该子网的所有路由器组" ...

- - - -

## IGMP Internet组管理协议
IGMP就是多播组里面成员通信的协议，类似加入退出某个多播组的通知，IGMP协议是多播技术的基本模块。

IGMP数据报
<pre>
  |<------ IP Datagram ----------->|
  |                                |
  +----------------------+---------+
  | IP Header            |IGMP Data|
  +----------------------+---------+
   20 Bytes               8 Bytes
</pre>

IGMP Data格式
<pre>
  0      3 4     7 8            15 16                            31
  +-------+-------+---------------+-------------------------------+-----
  |Version|Type   |Unused         |  Checksum                     |  |
  +-------+-------+---------------+-------------------------------+ 8 Bytes 
  | 32 bit Dtype IP Address                                       |  |
  +---------------------------------------------------------------+-----
</pre>

字段|意义
:--|:--
Version|版本号
Type|类型为1说明是路由器发出的查询报文; 为2说明是主机发出的报告报文
Dtype IP Addr|组地址是D类IP地址。查询报文时全为0


### IGMP使用规则

- 主机的第一个进程加入一个组是，主机就发一个IGMP报告, 多个进程加入同个组也只发送一个IGMP报告;
- 进程离开组时，主机不发送IGMP报文，就算组里面最后一个进程退出也不发，而是在随后的IGMP报文查询不再发送IGMP报文;
- 多播路由器定时发送IGMP查询来了解是否还有主机存在属于某个多播组的进程
- 主机如果还有属于某个多播组的进程，就要发送IGMP报告报文响应一个IGMP查询

- - - -

## DNS
域名系统(DNS)是一种用于TCP/IP应用程序的分布式数据库，提供主机名字和IP地址之间的转换及有关电子邮件的选路信息。

### DNS层次结构
DNS的名字空间具有如图的层次机构，与文件系统的路径差不多

<pre>
                                                                      
 Noname            +----+    +-------+   +---+   +---+   +--+   +--+  
  Root      +------|arpa|<---|in-addr|<--|140|<--|252|<--|13|<--|33| <-- 33.13.252.140.in-addr.arpa.
 +----+     |      +----+    +-------+   +---+   +---+   +--+   +--+
 |    |-----|
 +----+     |    -------------------------------------------------------------------------------
            |      +---+                                                                     |
            +------|com|<----                                                                |
            |      +---+                                                                     |
            |      +---+          +----+     +---+     +---+                                 |
            +------|edu|<---------|noao|<----|tuc|<----|sun|  <-----  sun.tuc.noao.edu       |
            |      +---+          +----+     +---+     +---+                                 |
            |      +---+                                                                     |
            +------|gov|<----                                                                |
            |      +---+                                                                   
            |      +---+                                                                   Normal
            |      +---+                                                                   domain
            |      +---+
            +------|mil|<----                                                                |
            |      +---+                                                                     |
            |      +---+                                                                     |
            +------|net|<----                                                                |
            |      +---+                                                                     |
            |      +---+                                                                     |
            +------|org|<----                                                                |
            |      +---+                                                                     |
            |     -------------------------------------------------------------------------------
            |    
            |     -------------------------------------------------------------------------------
            |      +--+                                                                      |
            +------|cn|                                                                      |
            |      +--+                                                                      |
            |       ..
            |      +--+           +---+     +------+     +----+                             Country
            +------|us|<----------|va |<----|reston|<----|cnri|  <----  cnri.reston.va.us   domain
            |      +--+           +---+     +------+     +----+
            |       ..                                                                       |
            |      +--+                                                                      |
            +------|zw|                                                                      |
                   +--+                                                                      |
                  -------------------------------------------------------------------------------
              |            |  |           |
              |            |  |           |
              | Top-Level  |  | Sec-Level |
                 domain          domain
</pre>

#### 顶级域名
顶层域名分为三个部份
1. arpa是用来IP地址到域名转换的特殊于; 
2. 7个3字符长的普通域(组织域)
3. 2字符长的国家域(ISO3166定义的国家代码)

### DNS报文
DNS报文一般被封装在UDP数据报中,只有在报文长度超过512字节，TC(删减字段)被设置上且截断，遇到这种情况，就需要用TCP重传了。

- - - -

## TFTP
TFTP(Trivial File Transfer Protocol)简单文本传输协议，最初打算用于引导无盘系统，TFTP使用UDP。TFTP的实现(UDP、IP和设备驱动程序)可以放入只读存储器中。

- - - -

## BOOTP 
BOOTP引导程序协议使用UDP，且通常需与TFTP协同工作，用于无盘系统进行系统引导。

### BOOTP服务器 "鸡和蛋的问题"
TFTP服务器如何將一个响应直接送回BOOTP客户? 因为发出这个UDP数据报的服务器可能向BOOTP客户的IP发送ARP请求，但这个时候客户机还不知道自己的IP！
解决方法:
- 服务器发一个ioctl(2)请求给内核，为该客户在ARP高速缓存中设置一个条目(客户机的链路地址在BOOTP请求里面);
- 广播解决~~

- - - -

## TCP 传输控制协议
TCP提供一种面向连接的、可靠的**字节流**服务。<br>
应用程序被分割成TCP认为最适合发送的数据块且数据报长度保持不变。由TCP传递给IP的信息单位成为报文段或段(segment)。<br>
TCP连接是全双工(即数据在两个方向能同时传递); 全双工也导致TCP的半关闭(half-close),因为双方能同时发送FIN段。

### TCP数据被封装在IP数据报
<pre>
   |<- IP Datagram ----------------------------------------------->|
   |                                                               |
   |                    |<---- TCP Segment ----------------------->|
   |                    |                                          |
   +--------------------+--------------------+--------...----------+
   | IP Header          | TCP Header         |  TCP Data           |
   +--------------------+--------------------+--------...----------+
    20 Bytes               20 Bytes
</pre>


### TCP首部
<pre>
  0                                                     15 16                                             31
  +-------------------------------------------------------+------------------------------------------------+
  |   16 Source Port                                      |   16 Destination Port                          |
  +-------------------------------------------------------+------------------------------------------------+
  |   32 Sequence Number                                                                                   |
  +--------------------------------------------------------------------------------------------------------+
  |   32 Confirm Sequence Number                                                                           |
  +------------+------------------+---+---+---+---+---+---+------------------------------------------------+
  |4Header Size| 6 Reserved       |URG|ACK|PSH|RST|SYN|FIN|  16 Windows Size                               |
  +------------+------------------+---+---+---+---+---+---+------------------------------------------------+
  |   16 Checksum                                         |   16 Urgent Pointer                            |
  +-------------------------------------------------------+------------------------------------------------+
  |                                                                                                        |
  z                                  Options                                                               z
  |                                                                                                        |
  +--------------------------------------------------------------------------------------------------------+
</pre>

### 标志位含义
标志|含义
:--|:--
URG|紧急指针(Urgent Pointer)有效
ACK|确认序号有效
PSH|接收方应该尽快將这个报文段发送给应用层
RST|重建连接
SYN|同步序号用来发起一个连接, 新建连接时，SYN标志为1
FIN|发送端完成发送任务

### Sequence Number
序号用来标识从TCP发端向TCP收端发送的数据字节流，是32bit的无符号数。新建连接时，序号字段设置为该连接的ISN(Initial Sequence Number)。ISN随时间而变化，所以每个连接的SYN段斗具有不同的ISN。


### Confirm Sequence Number
确认序号发送接受端期望收到的下一个序号，等于上次已收到的数据字节序号加1，。只有ACK标志为1确认序号才有效。

### 建立连接协议- 三次握手
客户端1个SYN段 + 服务器1个SYN段 + 客户端1个ACK段， 三次握手(three-way handshake)

1. 请求端(客户)发送一个SYN段说明客户打算连接的服务器的端口，以及初始化序号(ISN),为客户报文的序号值;
2. 服务器也发回包含服务器ISN(服务器报文段的序号值)的SYN段作为应答，同时报文段里确认序号设置为客户报文段的ISN+1,作为对客户SYN段的确认(+1表示SYN也占用一个序号);
3. 客户发回确认序号为服务器ISN+1的ACK段对客户端的SYN报文段进行确认;

### 终止连接协议- 四次握手协议
1. 关闭的一方发送第一个FIN段执行主动关闭;
2. 被关闭的一方收到FIN段之后，发回一个ACK段，ACK段的确认序号等于收到的FIN段的序号+1(+1表示FIN也占一个序号);
3. 被关闭的一方关闭连接，导致TCP端也向关闭的一方发送一个FIN段, FIN段的确认序号与前一个的ACK段的确认序号一样;
4. 关闭的一方对收到的FIN段也发送一个ACK段，ACK段的确认序号等于收到FIN段的序号+1;

### 最大报文段长度(MSS)
MSS表示TCP传往另一端的最大块数据的长度，可以用来避免分段。MSS选项只能出现在SYN段。

### TCP的半关闭(half-close)
TCP连接的一段在结束它的发送后(发了FIN段)还能接受来自另一端数据的能力，这就是半关闭。

### TCP状态变迁图
![TCP状态变迁图][pic_1]

### TIME\_WAIT
TIME\_WAIT状态也称为2MSL(Maximum Segment Lifetime)，从TCP状态变迁图可知TIME\_WAIT状态是已经发了FIN段且对另一端的FIN段回复了ACK段。MSL是任何报文被丢弃前在网络内的最长时间(RFC规定2分钟，一般为30秒或1分钟)。目的是让TCP可以在最后ACK丢失(另一端超时并重发最后的FIN)情况下再次发送ACK段。在2MSL期间，连接的socket不能再被使用。在2MSL期间，任何迟到的报文段將被丢弃。

### 复位报文段
TCP首部中的RST标识就是用于"复位"的
- 回复发到不存在的端口的连接请求。
> 比如客户1发送到服务1的TCP的SYN段(连接请求), 而目的端口没有进程在监听，服务1就会发送RST标识为1的ACK段回复
- 异常终止一个连接。
> 发送一个RST标识为1的ACK段来异常释放连接(abortive release)。收到复位ACK段不需要进行回复

### 半打开连接(half-open)
如果一方已经关闭或异常终止连接而另一方还不知道，这种TCP连接成为半打开的。

### 同时打开
两端同时发送创建连接的SYN段，这种情况建立连接过程如下：
1. 端1发送SYN1, 端2发送SYN2;
2. 端1收到SYN2, 回复ACK1; 端2收到SYN1，回复ACK2;
3. Established

### 同时关闭
两端同时主动关闭连接(发送FIN段)，这种情况建立连接过程如下：
1. 端1发送FIN1, 端2发送FIN2;
2. 端1收到FIN2, 回复ACK1; 端2收到FIN1，回复ACK2;
3. 结束连接

### 呼入连接请求队列
等待TCP连接请求的服务器会创建一个队列，用于存放已经建立连接(3次握手之后)的TCP连接, 等待应用程序处理队列里面的TCP连接。在队列满的时候，服务器不会对新的TCP连接请求进行应答(让客户端自己超时重新发送连接)。这种队列就是呼入连接请求队列。

### Nagle算法
TCP数据段中交互数据比重比较大的程序，比如Rlogin程序，通常一个分组只有一个字节数据:41字节的段(20字节IP首部+20字节TCP首部+1字节内容)。Nagle算法就是用来减少这种分组。<br>
该算法要求一个TCP连接最多只能有一个未被确认(未收到ACK段确认)的小分组，通过这样的限制，在ACK段确认到达之前，TCP可以收集这些少量的分组，在确认到达是再一起发出去。

### 滑动窗口
滑动窗口是TCP连接中接收端把自己的缓冲区大小告知发送端而达到控制发送端发送TCP数据段的速度。

#### TCP滑动窗口的可视化表示
途中的可用窗口就是此时发送端可以发送的报文大小！
![TCP滑动窗口的可视化表示][pic_2]

### PUSH标志
发送方使用PUSH标志通知接收方將所收到的数据全部提交给服务器接收进程，而不能等待判断是否还有额外的数据到达(服务器可能会等缓冲区填满再把数据提交给接收进程)。

### 慢启动
问题背景: 
> 发送方与接收方之间存在多个路由器和速率差异较大的链路时，连接速率差异大的路由器缓冲区可能会被快速的链路填满！<br>

解决方法:
> 慢启动(slow start)算法，使发送端的新分组进入网络的速率 与 接收端返回ACK确认速率 保持相同的算法！<br>

算法描述:
> 慢启动为发送方的TCP增加另一个窗口: 拥塞窗口(congestion window), 记为cwnd。<br>
> 1, 建立连接时，cwnd初始为1个报文段大小;<br>
> 2, 每收到一个报文段, cwnd增加1个报文段大小;<br>
> 发送方取拥塞窗口与通知窗口的最小值为发送上限;

- - - -

## TCP的定时器
TCP管理4个不同的定时器：
1. 重传定时器
> 用于希望收到另一端的确认
2. 坚持(persist)定时器
> 用于使窗口大小信息保持不断流动，即使另一端关闭了其接收窗口
3. 保活(keepalive)定时器
> 用于检测一个空闲连接的另一端何时崩溃或重启
4. 2MSL定时器
> 用于测量一个连接处于TIME\_WAIT的时间

- - - -
## TCP重传

### RTT与RTO
> RTT(round-trip time)往返时间: 用来衡量一个连接上的分组往返两端的时间，可以用来求去RTO的值。<br>
> RTO(Retransmission Timeout)重传超时时间：如果一个连接上的分组在RTO时间内没有收到确认段，就认为该分组丢失。<br>

#### RTO算法

#### 平滑算法
被平滑的RTT估计器(计为$R$):
$$R\leftarrow\alpha R + (1-\alpha)M$$
公式里面的$\alpha$是一个推荐值为0.9的平滑因子，$\leftarrow$右边的$R$表示前一个估计值，$M$表示新的RTT测量值。<br>
在以上$R$值基础上，通过一个推荐值为2的时延离散因子$\beta$来估算RTO:
$$ RTO = R\beta $$

#### 偏差算法
在RTT变化很大的情况下，平滑算法无法跟上变化。而可以基于测量的RTT的均值和方差来计算RTO
$$Err = M - A$$
$$A\leftarrow A + \scr{g} Err$$
$$D\leftarrow D + \scr{h} (|Err| - D)$$
$$RTO = A + 4D$$

A:          被平滑的RTT(均值的估算器)<br>
M:          每次RTT的测量值. <br>
Err:        当前测量值与当前的RTT估算器之差.<br>
$\scr{g}$:  起平均作用，取值为$\frac {1}{8}$. <br>
$\scr{h}$:  偏差的增益. 取值为$\frac {1}{4}$. <br>
A和D均被用于下一次重传时间RTO的计算。(A和D可以用一个固定的值进行初始化)

### Karn算法
当一个超时和重传发生时，重传数据的确认最后到达是，不能更新RTT估算器，因为无法判断这个确认段是对重传前的数据段还是重传的数据段的回复(重传的二义性)。

### 拥塞避免算法
拥塞避免算法是一种处理丢失分组的算法。<br>

两种分组丢失的标志：
- 发生超时(重传计时器跑满)
- 接到重复的确认(这种情况通常接收端收到了丢失报文段之后的报文)

拥塞避免算法和慢启动算法是两种目的不同、独立的算法。但是当拥塞发生时，希望通过减少分组进入网络的传输速率，于是可以通过调到慢启动来达到此目的。因此实际中两个算法通常在一起实现。

#### 拥塞窗口cwnd 和 慢启动門限ssthresh
算法流程：
1. 对于给定的连接，初始花cwnd为1个报文段，ssthresh为65535个字节;
2. TCP的输出不能超过cwnd和接放通知窗口的大小;
3. 拥塞发生时(超时或重复确认), ssthresh设置为当前窗口的一半(当前窗口为cwnd和接收通知窗口的较小者, 但至少为2个报文段). 此时如果是超时引起的拥塞，就把cwnd设置为1,进入慢启动;
4. 新的数据被确认时，根据当前的状态(拥塞避免状态 or 慢启动状态)对cwnd进行增加：
> 拥塞避免状态: cwnd > ssthresh ; 慢启动状态: cwnd $\leq$ ssthresh <br>
> 收到一个确认时cwnd的修改:<br>
> cwnd = cwnd + $\frac {1}{cwnd}$  (处于拥塞避免状态时，这样在一个RTT时间内cwnd最多增长为一个报文段)<br>
> cwnd = cwnd + 1 (处于慢启动时)

### 快速重传与快速恢复算法
在多个分组传输而只丢失了前面小部分的分组时，这时会收到多个重复ACK确认，一般一连串收到3个或以上的重复的ACK, 就可以认为发生报文丢失了而不需要等待超时定时器溢出而马上重传，这就是快速重传;而且接下来也不进入慢启动算法，依然拥塞避免算法，这就是快速恢复算法。

算法过程:
1. 当收到第3个重复的ACK时，將ssthresh设置为当前拥塞窗口cwnd的一半，重传丢失的报文。设置cwnd为ssthresh加上3个报文段的大小;
2. 每次收到1个ACK，cwnd就增加1个报文段大小并发送1个分组(如果新的cwnd允许的话);
3. 当重传的数据ACK到达时，设置cwnd为ssthresh.

- - - -

## TCP坚持定时器
Persist timer: 周期性地向接收方查询窗口是否增大而设计的计时器<br>
在连接的一方需要发送数据当对方已通知窗口大小为0时，就需要设置TCP的坚持计时器。

问题背景: 
> 由于TCP只确认那些包含有数据的ACK报文段，所以更新窗口的ACK报文如果丢失的话，发送窗口更新的数据接收方是无法确定该报文是否已经丢失。所以双方就有可能因为等待对方而使连接终止: 数据接收方等待接收数据(因为它已经向数据发送方发送了非0的窗口), 而数据发送方在等待允许它继续发送数据的窗口更新。<br>

解决方法: 
> 为了防止上面所述的死锁情况发生，数据发送方使用一个坚持定时器(persist timer)来周期性的向数据接受方查询，以便发现窗口是否已增大。这种报文段成为窗口探查(window probe)。

### 糊涂窗口综合征
糊涂窗口综合征(Silly Window Syndrome)是指基于窗口的流量控制方案导致的“少量的数据將通过连接进行交换”的状况，之前提到的Nagle算法就是解决TCP数据报太小的算法之一。<br>
可以在发送和接收两端采取措施避免出现SWS现象:
1. 接收方不通告小窗口。通常的算法接收方不通告一个比当前窗口大的窗口(可以为0),除非窗口可以增加一个报文段大小(MSS); 或者可以增加接收方缓存空间的1半，不论实际有多少。
2. 发送方在满足以下条件之一时才发送数据：
    - 可以发送一个满长度的报文段;
    - 可以发送至少是接收方通知窗口大小一半的报文段;
    - 能够发送手头所有数据并且不希望接受ACK;

- - - -

## TCP保活定时器块
保活定时器用于在连接空闲了两个小时之后，在一个连接上发送一个探查分组来完成保活功能。可能会发送4种情况：
1. 对端仍然运行正常;
2. 对端已经崩溃;
3. 对端已经崩溃并且重启(响应会进行复位);
4. 对端当前无法到达(中间的路由器崩溃之类); 


[pic_1]: ./TCP连接的状态变迁.png
[pic_2]: ./TCP滑动窗口可视化表示.png