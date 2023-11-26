# 问题：不是所有的网络都是直接想连的

由于以太网 Ethernet 只能连接不超过 10254 个设备，无线网络受限于无线电范围，所以为了实现互联网，需要连接不同种类的网络，即 *Internetworking* 
其中可以分为几个子问题
- *swithes/bridges/Layer 2 switches*: 用于 interconnect Ethernet segments，即连接**相同类型**的设备
- *gateways/routes/Layer 3 switches*: 用于连接**不同类型**的网络和连接

交换机工作在链路层，路由器工作在网络层

# 3.4 Routing

关于 Routing 的基本问题：switches 和 routers 怎样在 forwarding tables 中获得信息。

🔑Key Takeaway

1. forwarding (转发) 和 routing (路由) 的区别：
	- Forwarding：收到 packet 然后在表中查询地址并把其送到目的地。在每个节点本地执行。网络的*data plane*
	- Routing：forwarding table 建立的过程，依赖复杂的 distributed algorithms。网络的 *control plane*
2. forwarding table 和 routing table 的区别：
	- forwarding table：当 packet 被转发的时候转发表应该包含 network prefix 到 outgoing interface 的映射和一些 MAC 信息，利于 Ethernet address of the next hop。数据结构服务于转发时快速寻址。
	- routing table：通过 routing algorithms 建立的表格，是构建 forwarding table 的先驱。数据结构服务于计算拓扑结构的改变。

## 3.4.1 Network as a Graph

routing 的基本问题：寻找两个节点的 lowerst-cost path

## 3.4.2 Distance-Vector (RIP) 

两种更新路由的方式：定期更新和触发更新

count to infinity problem：
由于更新的交错，没有一个节点知道其中某一个节点已经不可访问了，此时网络的路由表不稳定。
解决办法之一：split horizon (with poison reverse) ，但是这只适用于两个节点的路由循环

## 3.4.3 Link State (OSPF)

Link-state routing 是第二大主要类内部路由协议。

Link-state protocols 背后的基本思想很简单：每个节点都知道如何直接到达相连的邻居，即每个节点都将有足够的网络知识来构建完整的网络地图。所以，link-state routing protocals 依赖两个主要机制：可靠的链接信息和根据所有积累的链路状态知识的总和来计算路由。

Reliable Flooding：每个节点会创建一个更新数据包，叫做（link-state packet, LSP），其中里面包含：计算 route 所需的参数，让洪泛可靠的信息。事实证明，让洪泛可靠非常困难。

Route Calculation：依赖 Dijkstra's 算法












