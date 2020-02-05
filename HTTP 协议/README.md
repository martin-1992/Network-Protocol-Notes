### HTTP 协议介绍
　　HTTP（HyperTextTransfer Protocol）为超文本传输协议，**确立了计算机之间进行交流的规范，即如何将超文本从一台机器传输到另一台机器的协议，通常运行在 TCP / IP 协议上。** 

- HTTP 协议的功能是传输超文本，域名解析需要 DNS 协议，寻找 IP 需要 IP 协议，[三次握手](https://github.com/martin-1992/Network-Protocol-Notes/blob/master/TCP%20%E5%8D%8F%E8%AE%AE/TCP%20%E7%9A%84%E4%B8%89%E6%AC%A1%E6%8F%A1%E6%89%8B.md)建立连接需要 TCP 协议，所以是运行在 TCP / IP 协议上。缺了这些，HTTP 协议是无法工作的；
- HTTP 协议有 1.1 和 2 两种，大部分为 1.1，默认开启 Connection: keep-alive。这样只需要第一次请求进行三次握手建立连接就行，后面的请求都不用再进行三次握手操作，因为三次握手很耗时。为此有很多程序都采取多路复用原则，即一个连接，多个 Channel，比如 Netty、[RabbitMQ 中在将生产者连接到 Broker 上](https://github.com/martin-1992/MQ-Notes/blob/master/RabbitMQ%20%E5%AE%9E%E6%88%98%E6%8C%87%E5%8D%97%E7%AC%94%E8%AE%B0/chapter_2/README.md)；
- HTTP 协议的传输是双向，并且在传输过程中允许中转，可添加其它功能，比如数据压缩、安全认证等；
- HTTP 协议传输的是超文本，载体为 HTML，包含文字、图片、视频、超链接等。

### [HTTP 请求的结构](https://github.com/martin-1992/Network-Protocol-Notes/blob/master/HTTP%20%E5%8D%8F%E8%AE%AE/HTTP%20%E8%AF%B7%E6%B1%82%E7%9A%84%E7%BB%93%E6%9E%84.md)
　　HTTP 请求的报文分为三部分，请求行、头部字段和实体正文。

![avatar](photo_1.png)

### [HTTP 返回的结构](https://github.com/martin-1992/Network-Protocol-Notes/blob/master/HTTP%20%E5%8D%8F%E8%AE%AE/HTTP%20%E8%BF%94%E5%9B%9E%E7%9A%84%E7%BB%93%E6%9E%84.md)
　　其中状态行，包含各种状态码，比如 200，表示请求正常处理。

![avatar](photo_3.png)

### [HTTP 请求的发送和返回](https://github.com/martin-1992/Network-Protocol-Notes/blob/master/HTTP%20%E5%8D%8F%E8%AE%AE/HTTP%20%E8%AF%B7%E6%B1%82%E7%9A%84%E5%8F%91%E9%80%81%E5%92%8C%E8%BF%94%E5%9B%9E.md)
　　HTTP 请求的发送和返回，都遵循[数据包封装和拆封流程](https://github.com/martin-1992/Network-Protocol-Notes/blob/master/%E7%BD%91%E7%BB%9C%E5%88%86%E5%B1%82/README.md)。在网络上传输的数据包，需是完整的，即包含 MAC、IP、TCP 这些。

### 面试题
　　面试题的内容来自 [你猜一个 TCP 连接上面能发多少个 HTTP 请求](https://zhuanlan.zhihu.com/p/61423830)，下面做了笔记整理。

#### 与服务器建立的连接是否会在一个 HTTP 请求后断开
　　用到了前面提到的 Connection: keep-alive。

- 在 HTTP/1.0 中，一个服务器在发送完一个 HTTP 响应后，会断开 TCP 连接。但每次请求都要重新建立和断开连接，代价过大（主要是来自初始化连接、 SSL 的和三次握手的开销），所以可以设置 Connection: keep-alive，这样发送完一个 HTTP 请求后不会断开连接；
- 在 HTTP/1.1 中，则设置默认的 Connection: keep-alive，即发送完一个 HTTP 请求后不会断开连接，不需要像 HTTP/1.0 那样需要先设置。只有设置 Connection: close 才会断开连接。

#### 一个 TCP 连接可以对应几个 HTTP 请求
　　如果是维持连接，即 Connection: keep-alive 的情况下，一个 TCP 连接可以发送多个 HTTP 请求。 注意，HTTP/1.1 无法并行处理，只能顺序处理多个连接请求，而 HTTP2 可以并行处理多个 HTTP 请求（加载图片这些）。<br />
　　因为建立和销毁连接的开销很大，所以通过维持连接来执行更多请求，避免了多次建立和销毁连接的开销，这在很多场景都有用到。比如 [RabbitMQ 中的通道复用多路连接](https://github.com/martin-1992/MQ-Notes/blob/master/RabbitMQ%20%E5%AE%9E%E6%88%98%E6%8C%87%E5%8D%97%E7%AC%94%E8%AE%B0/chapter_2/README.md)，通过在一个 TCP 连接上建立多个信道，可复用 TCP 连接，减少性能开销。

#### 一个 TCP 连接中 HTTP 请求发送可以一起发送么

- HTTP/1.1 是不可以的，单个 TCP 连接只能顺序发送请求，同一时刻只能处理单个请求。HTTP/1.1 有 Pipelining 技术可实现多个请求同时发送，但由于存在问题，所以默认关闭不使用。只有创建多个 TCP 连接，才能发送多个请求；
- HTTP2 中是可以同时发送多个请求的，因为有 Multiplexing 技术。

#### 为什么有的时候刷新页面不需要重新建立 SSL 连接
　　SSL 连接只要在 TCP 连接时才会，而 HTTP/1.1 默认设置 Connection: keep-alive，连接请求后会维持一段时间，即使发送完 HTTP 请求也不会立即断开。所以刷新页面时，不用 TCP 连接，自然就没有 SSL 连接的开销。

#### 浏览器对同一 Host 建立 TCP 连接的数量有没有限制？
　　是有限制，不同的浏览器限制数量不同，Chrome 最多允许对同一个 Host 建立六个 TCP 连接，当一个网页有几十张图片时，Chrome 最多能建立 6 个连接，并行加载 6 张图片。<br />
　　而如果是 HTTP2（需要在 HTTPS 上实现的），则可以使用 Multiplexing 并行处理多个请求，可以是多个 HTTP2 连接，每个 HTTP2 连接处理多个请求。<br />

### reference
- [你猜一个 TCP 连接上面能发多少个 HTTP 请求](https://zhuanlan.zhihu.com/p/61423830)
