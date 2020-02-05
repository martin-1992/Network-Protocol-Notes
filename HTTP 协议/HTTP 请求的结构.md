### HTTP 协议
　　HTTP 协议是基于 TCP 协议的，使用[三次握手](https://github.com/martin-1992/Network-Protocol-Notes/blob/master/TCP%20%E5%8D%8F%E8%AE%AE/TCP%20%E7%9A%84%E4%B8%89%E6%AC%A1%E6%8F%A1%E6%89%8B.md)建立 TCP 连接。HTTP 协议有 1.1 和 2 两种，大部分为 1.1，默认开启 Keep-Alive。<br />
　　这样只需要第一次请求进行三次握手建立连接就行，后面的请求都不用再进行三次握手操作，因为三次握手很耗时。为此有很多程序都采取多路复用原则，即一个连接，多个 Channel，比如 Netty、[RabbitMQ 中在将生产者连接到 Broker 上](https://github.com/martin-1992/MQ-Notes/blob/master/RabbitMQ%20%E5%AE%9E%E6%88%98%E6%8C%87%E5%8D%97%E7%AC%94%E8%AE%B0/chapter_2/README.md)。

### HTTP 请求的结构
　　HTTP 的报文分为三部分，请求行（start line）、头部字段（header）和实体（body）。

![avatar](photo_1.png)

#### 请求行
　　由三部分组成。

![avatar](photo_2.png)

- 请求方法，常用的有 GET、POST 等；
    1. GET，客户端请求从服务器获取资源，比如图片、页面等；
    2. POST，客户端将信息放在 HTTP 实体正文中，传给服务端；
    3. PUT，修改服务端的资源。
- 请求 URL，比如 https://www.google.com ；
- HTTP 版本，比如 HTTP 1.1、HTTP 2.0 等。

#### 头部字段
　　首部字段为 Key-Value，字段名为 Key，字段值为 Value。几个重要字段：

- Accept-Charset，告诉服务器客户端使用的编码字符集看，防止服务器返回使用其他编码，导致客户端解析不了，出现乱码；
- Content-Type，正文格式，常见的格式有 multipart/form-data、application/json、text/xml。一般使用 JSON 格式，即 Content-Type=application/json，实际根据场景需要选择；
- Range，指定请求资源的范围大小，比如一份 10000 字节大小的资源，使用 Range: bytes=5001-10000 表示请求 5001~10000 字节，用于下载中断后恢复下载。
- Cache-control，用于控制缓存。在高并发场景下，长时间不会变动的静态资源，比如图片等应该放在缓存中，防止每次请求都从服务器获取图片资源。另外，在大活动时，可通过模拟请求，预先将这些资源进行缓存。
    1. no-cache，先向服务器进行请求，如果没有变化，则使用缓存；
    2. no-store，请求和响应都禁止被缓存，每次都会向服务器发送请求，即不使用缓存；
    3. private，客户端可以使用缓存；
    4. public，客户端和代理器都可使用缓存；
    5. max-stale，缓存的内容将在 xxx 秒后失效，小于这段时间，则可以使用该缓存；
- Last-Modified，服务器在响应请求时，告诉浏览器资源的最后修改时间
- If-Modified-Since，与被请求资源的最后修改时间进行对比，资源修改时间小于 If-Modified-Since，则继续使用缓存，否则请求服务器获取最新修改的资源。

#### 实体正文
　　客户端请求的正文内容。
