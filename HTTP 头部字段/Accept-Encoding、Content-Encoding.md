### Accept-Encoding、Content-Encoding
　　请求头为 Accept-Encoding，客户端支持的压缩格式，比如 Accept-Encoding: gzip, deflate。没有该字段，则表示客户端不支持压缩数据。<br />
　　响应头 Content-Encoding，服务端会选择一种压缩格式，放在响应头 Content-Encoding 中并返回，客户端根据选择的压缩格式进行解压。没有该字段，表示响应数据没有被压缩。

#### Transfer-Encoding
　　响应头字段，告诉客户端，服务端发送内容的编码格式，可选值如下，可指定多个。

- Transfer-Encoding: chunked，数据分块发送。适用于发送图片、音频等这些已经高度压缩过的文件。Content-Length 为空，不知道其长度；
- Transfer-Encoding: gzip，使用 deflate 压缩算法 zlib 结构，适用于压缩文本文件；

　　chunked 是将大文件分成多个小文件来发送，发送过程中客户端和服务器的连接不会断开，为长连接。<br />
　　Content-Encoding 为传输编码，Content-Encoding，为内容编码。**对于一个消息正文，会进行内容编码，压缩内容，在进行传输编码，分块传输。** 内容编码和传输编码是相辅相成的，不存在冲突。
