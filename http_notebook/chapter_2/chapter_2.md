
## 简单的 HTTP 协议

### HTTP 协议用于客户端和服务器端之间的通信
　　HTTP 协议和 TCP/IP 协议族内的其他众多的协议相同，用于客户端和服务器之间的通信。请求访问文本或图像等资源的一端称为客户端，而提供资源响应的一端称为服务器端，而用 HTTP 协议能够明确区分哪端是客户端，哪端是服务器端。

### 通过请求和响应的交换达成通信
　　HTTP 协议规定，请求从客户端发出，最后服务器端响应该请求并返回。换句话说，肯定是先从客户端开始建立通信的，服务器端在没有接收到请求之前不会发送响应。

![Aaron Swartz](https://raw.githubusercontent.com/martin-1992/http_notebook/master/chapter_2/chapter_2_p1.png)

　　下面是从客户端发送给某个 HTTP 服务器端的请求报文中的内容。请求报文是由请求方法、请求 URI、协议版本、可选的请求首部字段和内容实体构成的。起始行开头的 "GET" 表示请求访问服务器的类型，称为方法（method）。随后的字符串 "/index.htm" 指明了请求访问的资源对象，也叫做请求 URI（request-URI）。最后的 "HTTP / 1.1"，即 HTTP 的版本号，用来提示客户端使用的 HTTP 协议功能。这段请求内容的意思是：使用 HTTP / 1.1 的协议，请求访问某台 HTTP 服务器上的 /index.htm 的页面资源。

![Aaron Swartz](https://raw.githubusercontent.com/martin-1992/http_notebook/master/chapter_2/chapter_2_p2.png)

　　接收到请求的服务器，会将请求内容的处理结果以响应的形式返回，如下图。响应报文基本上由协议版本、状态码（表示请求成功或失败的数字代码）、用以解释状态码的原因短语、可选的响应首部字段以及实体主体构成。在起始行开头的 HTTP/1.1 表示服务器对应的 HTTP 版本。紧挨着的 200 OK 表示请求的处理结果的状态码（status code）和原因短语（reason-phrase）。下一行显示了创建响应的日期时间，是首部字段（header field）内的一个属性。接着以一空行分隔，之后的内容称为资源实体的主体（entity body）。

![Aaron Swartz](https://raw.githubusercontent.com/martin-1992/http_notebook/master/chapter_2/chapter_2_p3.png)

### HTTP 是不保存状态的协议
　　HTTP 是一种不保存状态，即无状态（stateless）协议。HTTP 协议自身不对请求和响应之间的通信状态进行保存。也就是说在 HTTP 这个级别，协议对于发送过的请求或响应都不做持久化处理。即每当有新的请求发送时，就会有对应的新响应产生。协议本身并不保留之前一切的请求或响应报文的信息。这是为了
更快地处理大量事务，确保协议的可伸缩性，而特意把 HTTP 协议设计成如此简单的。<br />
　　HTTP / 1.1 虽然是无状态协议，但为了实现期望的保持状态功能，于是引入了 Cookie 技术。有了 Cookie 再用 HTTP 协议通信，就可以管理状态了。

### 请求 URI 定位资源
　　HTTP 协议使用 URI 定位互联网上的资源。正是因为 URI 的特定功能，在互联网上任意位置的资源都能访问到。当客户端请求访问资源而发送请求时，URI 需要将作为请求报文中的请求 URI 包含在内，指定请求 URI 的方式有很多。如果不是访问特定资源而是对服务器本身发起请求，可以用一个 \* 来代替请求 URI。如查询 HTTP 服务器端支持的 HTTP 方法种类，"OPTIONS * HTTP/1.1"。

![Aaron Swartz](https://raw.githubusercontent.com/martin-1992/http_notebook/master/chapter_2/chapter_2_p4.png)

### 告知服务器意图的 HTTP 方法
- **GET：获取资源。** GET 方法用来请求访问已被 URI 识别的资源。指定的资源经服务器端解析后返回响应内容。也就是说，如果请求的资源是文本，那就保持原样返回。如果是像 CGI（Common Gateway Interface，通用网关接口）那样的程序，则返回经过执行后的输出结果；

![Aaron Swartz](https://raw.githubusercontent.com/martin-1992/http_notebook/master/chapter_2/chapter_2_p5.png)

- **POST：传输实体主体。** POST 方法用来传输实体的主体。虽然用 GET 方法也可以传输实体的主体，但一般不用 GET 方法进行传输，而是用 POST 方法。虽说 POST 的功能与 GET 很相似，但POST 的主要目的并不是获取响应的主体内容；

![Aaron Swartz](https://raw.githubusercontent.com/martin-1992/http_notebook/master/chapter_2/chapter_2_p6.png)

- **PUT：传输文件。** PUT 方法用来传输文件。就像 FTP 协议的文件上传一样，要求在请求报文的主体中包含文件内容，然后保存到请求 URI 指定的位置。但是，鉴于 HTTP/1.1 的 PUT 方法自身不带验证机制，任何人都可以上传文件 , 存在安全性问题，因此一般的 Web 网站不使用该方法；

![Aaron Swartz](https://raw.githubusercontent.com/martin-1992/http_notebook/master/chapter_2/chapter_2_p7.png)

- **HEAD：获得报文首部。** HEAD 方法和 GET 方法一样，只是不返回报文主体部分。用于确认 URI 的有效性及资源更新的日期时间等；

![Aaron Swartz](https://raw.githubusercontent.com/martin-1992/http_notebook/master/chapter_2/chapter_2_p8.png)

- **DELETE：删除文件。** DELETE 方法用来删除文件，是与 PUT 相反的方法。DELETE 方法按请求 URI 删除指定的资源。但是，HTTP/1.1 的 DELETE 方法本身和 PUT 方法一样不带验证机制，所以一般的 Web 网站也不使用 DELETE 方法；

![Aaron Swartz](https://raw.githubusercontent.com/martin-1992/http_notebook/master/chapter_2/chapter_2_p9.png)

- **OPTIONS：询问支持的方法。** OPTIONS 方法用来查询针对请求 URI 指定的资源支持的方法；

![Aaron Swartz](https://raw.githubusercontent.com/martin-1992/http_notebook/master/chapter_2/chapter_2_p10.png)

- **TRACE：追踪路径。** TRACE 方法是让 Web 服务器端将之前的请求通信环回给客户端的方法；

![Aaron Swartz](https://raw.githubusercontent.com/martin-1992/http_notebook/master/chapter_2/chapter_2_p11.png)

![Aaron Swartz](https://raw.githubusercontent.com/martin-1992/http_notebook/master/chapter_2/chapter_2_p12.png)

- **CONNECT：要求用隧道协议连接代理。** CONNECT 方法要求在与代理服务器通信时建立隧道，实现用隧道协议进行 TCP 通信。主要使用 SSL（Secure Sockets Layer，安全套接层）和 TLS（Transport Layer Security，传输层安全）协议把通信内容加密后经网络隧道传输。

![Aaron Swartz](https://raw.githubusercontent.com/martin-1992/http_notebook/master/chapter_2/chapter_2_p13.png)

　　下表列出了 HTTP/1.0 和 HTTP/1.1 支持的方法。另外，方法名区分大小写，注意要用大写字母。

![Aaron Swartz](https://raw.githubusercontent.com/martin-1992/http_notebook/master/chapter_2/chapter_2_p14.png)

### 持久连接节省通信量
　　HTTP 协议的初始版本中，每进行一次 HTTP 通信就要断开一次 TCP连接。如果浏览器访问的一个包含多张图片的 HTML页面时，则需要进行多次请求，增加通信量的开销。为解决这一问题，提出了持久连接，其特点：
- 只要任意一端没有明确提出断开连接，则保持 TCP 连接状态。即在建立 1 次 TCP 连接后进行多次请求和响应的交互。持久连接的好处在于减少了 TCP 连接的重复建立和断开所造成的额外开销，减轻了服务器端的负载。另外，减少开销的那部分时间，使 HTTP 请求和响应能够更早地结束，这样 Web 页面的显示速度也就相应提高了。在 HTTP/1.1 中，所有的连接默认都是持久连接，但在 HTTP/1.0 内并未标准化；
- 持久连接使得多数请求以管线化（pipelining）方式发送成为可能，即可并发发送多个请求，而不需要一个接一个地等待响应了。

![Aaron Swartz](https://raw.githubusercontent.com/martin-1992/http_notebook/master/chapter_2/chapter_2_p15.png)

### 使用 Cookie 的状态管理
　　HTTP 是无状态协议，它不对之前发生过的请求和响应的状态进行管理。也就是说，无法根据之前的状态进行本次的请求处理。假设要求登录认证的 Web 页面本身无法进行状态的管理（不记录已登录的状态），那么每次跳转新页面不是要再次登录，就是要在每次请求报文中附加参数来管理登录状态。<br />
　　保留无状态协议这个特征的同时又要解决类似的矛盾问题，于是引入了 Cookie 技术。Cookie 技术通过在请求和响应报文中写入 Cookie 信息来控制客户端的状态，流程：
- 客户端发送请求；
- 服务端生成 Cookie，在发送的响应报文内的一个叫做 Set-Cookie 的首部字段信息，添加 Cookie，通知客户端保存 Cookie；
- 下次客户端会自动在请求报文中加入 Cookie 后发送；
- 服务端发现客户端发送过来的 Cookie 后，会去检查究竟是从哪一个客户端发来的连接请求，然后对比服务器上的记录，最后得到之前的状态信息。

![Aaron Swartz](https://raw.githubusercontent.com/martin-1992/http_notebook/master/chapter_2/chapter_2_p16.png)

![Aaron Swartz](https://raw.githubusercontent.com/martin-1992/http_notebook/master/chapter_2/chapter_2_p17.png)

　　HTTP 请求报文和响应报文的内容如下。

![Aaron Swartz](https://raw.githubusercontent.com/martin-1992/http_notebook/master/chapter_2/chapter_2_p18.png)
