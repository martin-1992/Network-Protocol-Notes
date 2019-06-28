
### HTTP 协议
　　HTTP 协议是基于 TCP 协议的，使用[三次握手](https://github.com/martin-1992/Network-Protocol-Notes/blob/master/TCP%20%E5%8D%8F%E8%AE%AE/TCP%20%E7%9A%84%E4%B8%89%E6%AC%A1%E6%8F%A1%E6%89%8B.md)建立 TCP 连接。HTTP 协议有 1.1 和 2 两种，大部分为 1.1，默认开启 Keep-Alive。这样只需要第一次请求进行三次握手建立连接就行，后面的请求都不用再进行三次握手操作，因为三次握手很耗时。<br />
　　为此有很多程序都采取多路复用原则，即一个连接，多个 Channel，比如 Netty、[RabbitMQ 中在将生产者连接到 Broker 上](https://github.com/martin-1992/MQ-Notes/blob/master/RabbitMQ%20%E5%AE%9E%E6%88%98%E6%8C%87%E5%8D%97%E7%AC%94%E8%AE%B0/chapter_2/README.md)。

### [HTTP 请求的结构](https://github.com/martin-1992/Network-Protocol-Notes/blob/master/HTTP%20%E5%8D%8F%E8%AE%AE/HTTP%20%E8%AF%B7%E6%B1%82%E7%9A%84%E7%BB%93%E6%9E%84.md)
　　HTTP 请求的报文分为三部分，请求行、首部和实体。

### [HTTP 返回的结构](https://github.com/martin-1992/Network-Protocol-Notes/blob/master/HTTP%20%E5%8D%8F%E8%AE%AE/HTTP%20%E8%BF%94%E5%9B%9E%E7%9A%84%E7%BB%93%E6%9E%84.md)
　　其中状态行，包含各种状态码，比如 200，表示请求正常处理。

### [HTTP 请求的发送和返回](https://github.com/martin-1992/Network-Protocol-Notes/blob/master/HTTP%20%E5%8D%8F%E8%AE%AE/HTTP%20%E8%AF%B7%E6%B1%82%E7%9A%84%E5%8F%91%E9%80%81%E5%92%8C%E8%BF%94%E5%9B%9E.md)
　　HTTP 请求的发送和返回，都遵循[数据包封装和拆封流程](https://github.com/martin-1992/Network-Protocol-Notes/blob/master/%E7%BD%91%E7%BB%9C%E5%88%86%E5%B1%82/README.md)。在网络上传输的数据包，需是完整的，即包含 MAC、IP、TCP 这些。
