### HTTP 返回的结构

![avatar](photo_3.png)

### 状态行
　　由三部分组成，分别为 HTTP 版本、状态码、原因组成。

![avatar](photo_4.png)

- **HTTP 版本。** 比如 HTTP 1.1、HTTP 2.0 等；
- **状态码。** 由三位数字组成，数字第一位指定了响应类别，有 5 种响应类别，具体可看 [返回结果的 HTTP 状态码](https://github.com/martin-1992/Network-Protocol-Notes/blob/master/http_notebook/chapter_4/chapter_4.md)；
  1. 1XX，Informational（信息性状态码），表示接收的请求正在处理；
  2. 2XX，Success（成功状态码），表示请求正常处理完毕；
  3. 3XX，Redirection（重定向状态码），表示需要进行附加操作以完成请求；
  4. 4XX，Client Error（客户端错误状态码），表示服务器无法处理请求；
  5. 5XX，Server Error（服务器错误状态码），表示服务器处理请求出错。
- 原因，补充状态码说明原因。

### 首部字段
　　返回的首部字段也是 Key-Value 形式，字段名 Content-Type，值可为 HTML 或 JSON，表示返回的格式。

### 实体正文
　　服务端根据请求返回的正文内容。
