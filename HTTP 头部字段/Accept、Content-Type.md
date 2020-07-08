### Accept、Content-Type
　　请求头为 Accept，告诉服务端，客户端能够接收的内容类型，比如 Accept: image/png, text/html, application/xml 等。<br />
　　响应头为 Content-Type，服务端返回该内容的 MIME 类型，比如 Content-Type: text/html; charset=utf-8。客户端看到 "text/html"，知道是 html 文件，会渲染出页面。

#### MIME 类型
　　HTTP 用于标记消息正文 body 的数据类型，这样客户端和服务端才知道是什么数据，使用什么来解析 body 的内容。

- **text，文本格式的可读数据。** 比如超文本文档 text/html、纯文本 text/plain、样式表 text/css 等；
- **image，图像文件。** 有 image/gif、image/jpeg、image/png 等；
- **audio/video，音频和视频数据。** 比如 audio/mpeg、video/mp4 等；
- **application，数据格式不固定，由上层应用程序来确定。** 比如 application/json，application/javascript、application/pdf 等；
