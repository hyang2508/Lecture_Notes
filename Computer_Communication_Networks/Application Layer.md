
## 12_Applications_and_the_DNS

>[!summary] Application Identifiers
>- Application layer typically involves connecting to a host identified by a domain name.
>	- Human readable term, maps "under the hood" to an IP address.
>	- Large, hierarchical, global, cooperative system performs name -> address mapping.
>- Different applications run over different port numbers.
>	- Well known ports for servers; ephemeral ports for clients.
>	- Ports can be "hacked" to allow multiple users to share an IP address using network address translation.

### Ports

>[!summary] Ports
>- Once a packet has reached the right host, a port number tells the operating system which application to hand the packet to.
>- Port numbers are used — independently — in UDP and TCP
>- Servers "listen" on ports with a "well-known" number for clients to find them.
>- Clients use "ephemeral ports" that only endure for the connection.
>- NATs exploit port numbers to expand access to IP.

Servers "listen" on ports with a "well-known" number for clients to find them. Clients use "ephemeral ports" that only endure for the connection. 

>[!example] well-known ports 
>- The modern secure web runs over port 443
>- Older (unencrypted) web runs over port 80
>- Email, by default, uses port 25
>- SSH uses port 22

可以通过下面的命令查看电脑现在有哪些 ports 是活跃的，它们连接了哪些应用

>[!info] 查看port的指令
>- OSX: `netstat -anvp tcp | less`
>- Windows: `netstat -ab`
>- Linux: `netstat -tulpn`


一旦 packet 抵达了正确的 host，a port number 会告诉操作系统应该将这个 packet 送往哪个应用。

端口还有一些更酷的应用，例如解决 IP address exhaustion 的问题

>[! info] IP address exhaustion
>- 长期措施：使用 IPv6 地址（128 位）
>- 短期措施：NAT (Network Address Translation)

NAT 的核心思想：每个 IP 地址可以有最多 $2^{16}$ 个 Ports，clients 可以共享同样的 IP 地址，但是使用不同的 port numbers 来区分。最多可以有 $2^{32}$ 个 TCP src/dst 端口对

在组织内部会分配每个 host 一个私有 IP 地址，例如 10/8 和 192.168/16

>[!attention]
>- NAT 的好处：显著减少对公有 IP 地址的需求，同时促进了安全性
NAT 的缺点：Fate Sharing，一旦 NAT 失效，所有的 communication sessions 都会丢失

### Domain Name System (DNS) - Finding the Host

>[!summary] Domain Name System
>- Easy unique, human-readable naming
>- Hierarchy helps with scalability
>- Caching lends scalability, performance
>- Indirection enables more scalability, load balancing, flexibility

Domain Names 的好处：

- 便于人类记忆
- 域名在客户端和服务器之间提供"间接"一层，可以以有趣的方式进行操纵…
	- 如果给一个 DNS 名称多个 IP 地址
		- Load Balancing
		- Performance Optimization
		- Migration/Upgrades（当切换 server 的时候不需要告诉 client 新地址）
	- 如果给一个 IP 地址多个 DNS 名称
		- 可以在同一物理上托管不同的命名服务。例如很多云服务商在同样的机器上部署了大量网站，这些网站都有同样的 IP 地址和端口。当 client 请求一个网站的时候，网站 server 会加载它们请求的网站。（Multiplex）

早期，一个名叫 SRI 的机构管理域名到 IP 地址的映射，这个文件叫"hosts"，所有的 server 都需要定期下载最新的"hosts"文件的拷贝。

>[! note] 我们的电脑上仍然存在这个文件
>- Linux：`/etc/hosts`
>- Windows：`C:\Windows\System32\drivers\etc\hosts`

但是随着互联网的增长，这个方式由于不能拓展而被抛弃。

拓展 DNS 的目标：

- Scalable
	- many names
	- many updates
	- many users creating names
	- many users looking up names
- Highly available
- Correct
	- no naming conflicts (uniqueness)
	- consistency
- Lookups are fast

核心思想：hierarchical distribution

- Hierarchical namespace
	- Human/Organizational Hierarchy in the DNS
	- 例如： `www.ece.ust.hk` 
		- **Top level domains**: globally recognized organizers and historically might represent countries (`.uk` for the United Kingdom), classes of services (`.edu` for education), or infrastructure (`.arpa`).
		- **Domain names**: can be registered (usually purchased) from the authority managing a top-level domain (TLD). The `.edu` organization only allows educational institutions to have domains within their TLD. 如果个人想买域名需要通过注册服务商，如 [Buy a domain name - Namecheap](https://www.namecheap.com/)
		- **Subdomains**: can be created however the domain administrator wants to. Here, HKUST is using subdomains to reflect different academic units — ECE has its own subdomain (ece. ust. hk) and CSE has another (cse. ust. hk).
			- Subdomains can even be divided into even smaller subdomains (maybe we should call them sub-subdomains ;-). This subdomain doesn't reflect organizational hierarchy, but usage — we are saying that we want to access the server, in the ECE department, that runs the website — hence the www.
- Hierarchically administered
	- Each name server can be managed by a different organization
- Hierarchy of servers

![image.png](https://image-host-1316314535.cos.ap-beijing.myqcloud.com/Obsidian%20Image/DNS%20Design.png)

DNS Root 在所有 Zone 的顶部，DNS Root Servers 一共有 13 个，可以在 [Root Server Technical Operations Association](https://root-servers.org/) 上查询。

Each server is replicated via any-casting

>[!note] Anycast
>Routing finds shortest paths to destination. The network will deliver the packet to the closest machine with that address. This is called "anycast".
>- very robust
>- Requires no modification to routing algorithms

根并不存储所有域名，树中"较高"的名称服务器的主要工作只是提供指向树中"较低"名称服务器的指针。只有 authoritative name servers 才能真正保持域名到 IP 地址的映射。

>[!example]
>在 linux 中执行命令 `dig SOA @m.root-servers. net www.ece.hkust.edu.hk` 可以查询域名 `www.ece.hkust.edu.hk` 在 `m.root-servers.net` 根域名服务器上的 SOA（Start of Authority）记录。

>[!tip] 域名的官方文档
>- RFC 1034 -Domain names -concepts and facilities : [RFC 1034 - Domain names - concepts and facilities](https://datatracker.ietf.org/doc/html/rfc1034)
>- RFC 1035 -Domain names -implementation and specification:[RFC 1035 - Domain names - implementation and specification](https://datatracker.ietf.org/doc/html/rfc1035)

Some DNS nitty gritty…

Using DNS (Client/App View)：通常，您的客户端不会向许多服务器发出 DNS 请求，相反，一般联系默认的 DNS 服务器或解析程序。这个 DNS 服务器在本地网络中，可以通过 DHCP （提供 IP 地址和网关的协议）查找默认 DNS 服务器

Benefit of Asking a Resolver: DNS Caching，即缓存所有级别的 DNS 响应


## 13_The_Web

### Hypertext System
 
在 WWW 之前有许多其他超文本系统

Why is hypertext so powerful? transformed our mentality of Internet use from "point to point" communications (email, files on a single server) to one of exploration of all of the information online.

#### Enabling Hypertext: The URL 

URLs (Uniform Resource Locator) are a subclass of a more general "Uniform Resource Identifier" 

- 对网络上的资源指定的引用
	- 主机在网络中的位置 (authority)
	- 资源在主机上的位置 (path)
	- 用于检索的协议 (scheme)

>[!example]
> `https://ece.hkust.edu.hk/index.php`
> - `https://`：使用 https 协议访问的网页
> - `ece.hkust.edu.hk`：域名或 IP 地址，也可以手动指定端口号，如`:443`
> - `/index.php`：有关在何处查找您正在联系的服务器上的文件的信息的路径

#### Using Hypertext: HTML

Originally, HTML is a simple language for displaying
text, images, and hypertext; to be used in the web. Today, a much richer language that orchestrates multiple different languages and files to generate web applications. (e.g. cascading style sheets, JavaScript)

Loading a "Single Web Page"Means Loading Many Files! Because scripts and Stylesheets are invisible.

### HTTP - The Hyper Text Transfer Protocol

- Client-server architecture
	- Server is "always on" and "well known"
	- Clients initiate contact to server
- Synchronous request/reply protocol
	- Client asks sends a request, server replies

![image.png](https://image-host-1316314535.cos.ap-beijing.myqcloud.com/Obsidian%20Image/http%20message.png)

- HTTP 有一些很疯狂的想法
	- HTTP 是纯文本，可以直接阅读
	- HTTP 是可变长度，没有固定字节偏移
- 导致软件中更复杂的解析
	- 但是，人类更容易推理
	- 可拓展

>[!note] HTTP is Stateless
> - 每一个 request-response 都被独立对待
> 	- 服务器不需要保留状态
> 	- 抽象只是"请求和接收文件"
> - 优点：提高服务器端的可拓展性
> 	- 可以很容易处理 Failure，处理高并发请求，不在意请求顺序
> - 缺点：不适合需要持续状态的应用
> 	- 需要唯一识别用户或存储临时信息
> 
> 解决措施：push state elsewhere
> - **Client: Cookies.** web browser keeps some identifiers (e.g., an authentication token) or state and returns this information to the web server with its request.
> - **Server: Database.** store information relevant to client history here; when you receive a request from a client with a cookie, look up the right information from the database before assembling your response

>[!warning]
>some pages will take longer to assemble than others. 即 PLT（page load time）更长。

#### HTTP Performance

Most Web pages have multiple objects. If you just "GET" one per TCP connection, it will cause lots of handshakes & congestion control state lost across connections. (HTTP/1.0: 每次请求都需要重新建立一次连接)
 
Optimizing measure: Persistent connections

- Maintain TCP connection across multiple requests
	- Including transfers subsequent to current page
	- Client or server can tear down connection
- Performance advantages
	- Avoid overhead of connection set-up and tear-down
	- Allow TCP congestion window to increase

Can we go faster? **Pipelined** Requests & Responses

- Key idea: don't need to wait until the previous request has received a response to send the next request.
- Problems: Head of Line Blocking

Three Generations of "Fixes" to HOL Blocking

- Generation #1 : HTTP/1.1 Concurrent/Parallel Requests & Responses (并发请求和响应) Over Parallel TCP Sessions
	- 允许持续连接，但是每个 request 内不允许乱序处理。但是可以有不同的连接乱序处理不同的 request。
	- 这对 Client 和 Content provider 是好的，但是对 Network 是不公平的，因为一个请求可以开多个 Request 占据网络资源，使得其他人无法使用网络。
- Generation #2 : HTTP/2.0 Streams
	- Still uses one persistent connection for multiple requests
	- Request/Response pairs now abstracted into the idea of a "stream"
		- Each request/response is labeled with a Stream ID
		- Server is free to send responses for different streams out of order
		- This avoids HOL blocking since "fast" objects can be sent right away while processing continues for "slow" objects
	- Still have problem: HTTP 2.0 fixes HOL at Layer 7, but not at Layer 4
		- Missing packet for Stream 1 prevents client from reading in data for Stream 2 in Receive Buffer!
- Generation #3 : HTTP/3.0 with UDP (QUIC)
	- Persistent connections + pipelining + re-orderable streams…
	- Allows application to pull in data for streams as soon as data for that stream is complete, even if data for other streams has been lost in the network.
	- Quick UDP Internet Connections (QUIC)

参考资料：[HTTP 1.0、1.1、2.0、3.0区别 - 简书](https://www.jianshu.com/p/cd70b8e90d00)

>[!summary] HTTP Evolution
>The previous tips are just a "highlights reel" of big ideas in the evolution of HTTP to make pages load faster.Why teach these ideas, and not other things in HTTP?
>
>Turns out that these concepts highlighted — pipelining, parallelism, out-of-order processing — all appear throughout computer systems. You might see versions of these ideas in architecture or databases too.
>- **Persistent connections** to avoid setup/teardown overheads
>- **Pipelining/windowing** to keep lots of requests/responses "in-flight"
>- **Concurrent connections** can help with re-ordering, but come with their own drawbacks
>- **Out-of-order processing** solves head-of-line blocking and improves work conservation.

### More Ideas to Make the Web Fast: Caching

Key idea: Lots of users accessing the same content. Store a local "cached" copy of popular content near clients so that they don't have to spend a full RTT accessing the data.

So pages load faster and network operators use less bandwidth accessing the same content over and over again.

#### Where?

Baseline: Many clients transfer the same information
An ideal cache is: Shared by many clients and very close to the client

- Caching with Clients
	- Clients keep a local cache of recently accessed objects
	- Cheap: no additional infrastructure needed
	- But caching closer to server can lead to higher hit rates!
- Caching with Reverse Proxies
	- Cache documents close to server $\rightarrow$ decrease server load
	- Typically done by content provider
- Caching with Forward Proxies
	- Cache documents close to clients $\rightarrow$ Decrease latency
	- Typically done by ISPs or enterprises $\rightarrow$ Reduce provider traffic load
	- Very cost effective

#### How to Avoid Stale Content?

- Modifier to GET requests
	- If-modified-since – returns "not modified" if resource not modified since specified time
- Response header
	- Expires – how long it's safe to cache the resource. 
	- No-cache – ignore all caches; always get resource directly from server

### Replication

- Content providers can explicitly replicate popular Web site across many machines spread across the globe
	- Balances out load across servers
	- Places content closer to clients, like caches do — but now provider controls cache contents
- How do we direct clients to the right copy of the data?
	- CDNs and Content Distribution

## 14_Content_Delivery_Network















## 18_Network_Security



## 19_Real_Time_Video