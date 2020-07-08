### Last-modified
　　响应头字段，请求资源的最后修改时间，比如 Last-Modified: Tue, 15 Nov 2010 12:45:26 GMT。<br />
　　客户端通过发送 head 请求来获取最新的修改时间 Last-modified，在对比上次的 Last-modified，如果时间不对，表示有修改。则不使用缓存，而是从服务端重新请求获取新的。

### If-Modified-Since
　　请求头字段，表示该文件上一次修改的时间，配合响应头 Last-modified 一起使用，Last-modified 的值会保存到 If-Modified-Since。

- 浏览器发送请求，服务端收到请求后，会返回资源和响应头，其部分为 Last-Modified: 2019-03-01 13:00:00、Cache-Control: max-age=50；
- 客户端保存为缓存，将 Last-Modified 的值保存为 If-Modified-Since，If-Modified-Since: 2019-03-01 13:00:00；
- 当 20 秒后在次请求，由于 2019-03-01 13:00:20 < 2019-03-01 13:00:50，没到过期时间，所以浏览器会从缓存中获取资源；
- 只有当过期后，发的请求才会到服务端，由服务端验证资源是否更新，是否要重新获取缓存；
- 如果没有更新的话，服务端会返回状态码 304 (Not Modified)，表示可以继续使用缓存。