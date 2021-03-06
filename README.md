## Network-Protocol-Notes

### [ARP 协议](https://github.com/martin-1992/Network-Protocol-Notes/tree/master/ARP%20%E5%8D%8F%E8%AE%AE%E8%8E%B7%E5%8F%96%20MAC%20%E5%9C%B0%E5%9D%80)
　　通过广播的方式来寻找目标 MAC 地址的，即已知 IP 地址，求 MAC 地址的协议。获取地址后，会使用缓存存放一段时间，这样就不用每次都要广播了。

### [DNS 协议](https://github.com/martin-1992/Network-Protocol-Notes/tree/master/DNS%20%E5%B7%A5%E4%BD%9C%E6%B5%81%E7%A8%8B)
　　用于域名解析，获取 IP。按照域名，从上到下分配对应的 DNS 服务器。域名解析可返回多个 IP 地址（主机），可做负载均衡。<br />
　　以 "https://www.google.com/" 为例，使用 "." 进行分割，最右边的 "com" 为顶级域名，"google" 为二级域名，层级逐渐递减，"www" 为主机名。

![avatar](./DNS%20工作流程/photo_1.png)

　　在微服务下，**负载均衡是通过注册中心获取一个 IP 列表，通常在客户端代码上进行负载均衡，** 选取一个提供者。优点是能快速上下线新 IP 主机，遇到突发流量时，也能通过批量上线一批新 IP，应该突发情况。

### [IP 地址和 MAC 地址](https://github.com/martin-1992/Network-Protocol-Notes/tree/master/IP%20%E5%92%8C%20MAC)

- IP 地址分为 IPv4 和 IPv6 两种；
- MAC 地址是一个网卡的物理地址，用十六进制，6 个 byte 表示。

### [TCP / IP 协议](https://github.com/martin-1992/Network-Protocol-Notes/tree/master/TCP-IP%20%E5%8D%8F%E8%AE%AE)
　　为多个网络通信协议的统称，有四层。由下至上，分别为链路层、网络层（IP 协议）、传输层（TCP 协议）、应用层（HTTP 协议、FTP 协议等），缺一不可。

![avatar](./TCP-IP%20协议/photo_2.png)

#### 网络层

- IP 协议，通过 IP 协议，使用 IP 地址，可跨局域网定位到某台机器设备；
- [ICMP 协议](https://github.com/martin-1992/Network-Protocol-Notes/tree/master/ICMP%20%E4%B8%8E%20PING%20%E6%B5%81%E7%A8%8B%E8%A7%A3%E6%9E%90)，即互联网控制报文协议，ICMP 报文是封装在 IP 包里。在遇到问题传输命令时，需要源地址和目标地址，PING 是基于 ICMP 协议工作的。

#### 传输层

- [TCP 协议](https://github.com/martin-1992/Network-Protocol-Notes/tree/master/TCP%20%E5%8D%8F%E8%AE%AE)，使用三次握手建立连接，保证两台设备之间进行可靠传输；
- [UDP 协议](https://github.com/martin-1992/Network-Protocol-Notes/tree/master/UDP%20%E5%8D%8F%E8%AE%AE)，一个完整数据包为 [MAC 头，IP 头，UDP 头，HTTP 内容，HTTP 正文]。其中 UDP 头包含源端口、目标端口号、UDP 长度、UDP 校验和以及数据。

#### 应用层

- [HTTP 协议](https://github.com/martin-1992/Network-Protocol-Notes/tree/master/HTTP%20%E5%8D%8F%E8%AE%AE)，HTTP（HyperTextTransfer Protocol）为超文本传输协议，确立了计算机之间进行交流的规范，即如何将超文本从一台机器传输到另一台机器的协议。HTTP 是传输协议，寻找 IP、建立连接这些则是由下层来处理，比如 TCP/IP 协议，所以说 HTTP 协议运行在 TCP / IP 上；

### [HTTP 协议的头部字段](https://github.com/martin-1992/Network-Protocol-Notes/tree/master/HTTP%20%E5%A4%B4%E9%83%A8%E5%AD%97%E6%AE%B5)

- Accept、Content-Type，为数据类型字段，表示发送、接收的数据包类型；
- Accept-Charset、Content-Type，表示数据编码的方式，常用的编码为 UTF-8、gbk，用于对消息正文进行编码；
- Accept-Encoding、Content-Encoding，为数据压缩字段，用什么格式进行压缩；
- Accept-Language、Content-Language，为数据语言字段，即客户端和服务端协商使用什么语言进行展示，比如中文 zh-Cn、英文 en 等；
- Range、Content-Range，为范围请求字段，用于获取部分数据（文件一部分），比如跳过片头、多段下载、断点续传；
- Cookie、Set-Cookie，让无状态的 HTTP 可以记忆，存储在客户端（浏览器）；
- Cache-Control，请求头字段和响应头字段都有 Cache-Control，两边都可以使用，用于设定服务端和客户端的缓存控制；
- Via，请求头字段和响应头字段都有 Via，代理服务器用该字段来表明代理身份。当一个数据包经过一个代理服务器时，会在 Via 字端结尾追加该代理服务器的代理主机名或域名；
- Last-modified、If-Modified-Since，为条件请求字段，资源时间的修改；
- ETag、If-None-Match，资源的唯一标识；
- Connection: keepalive，请求头字段，默认使用长连接；
- Location，响应头字段，要跳转的地址，配合 301（永久重定向）/ 302（临时重定向）使用。

### reference

- [趣谈网络协议](https://time.geekbang.org/column/intro/85)；
- [透视 HTTP 协议]()
