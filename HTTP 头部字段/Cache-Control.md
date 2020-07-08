### Cache-Control
　　请求头字段和响应头字段都有 Cache-Control，两边都可以使用，有以下几个属性。

- **Cache-Control: max-age=10，** Cache-Control 字段里的 max-age 和前面讲的 Cookie 里的 max-age 类似，都是标记资源的有效期；
    1. 区别在于计算有效期的起始不同，Cache-Control 的 max-age 是从响应报文创建开始计算，即离开服务器的时刻，Cookie 的 max-age 是从客户端收到报文的时刻开始计算；
    2. 比如，Cache-Control: max-age 设定为 10，但因为网络问题，在链路上堵了 8 秒，最后到达客户端，存储在浏览器中只有 2 秒。
- **Cache-Control: no-store，** 禁止浏览器使用缓存，用于个人隐私数据、银行业务数据的响应、频繁变化的页面（秒杀页面）；
- **Cache-Control: no-cache，** 在使用缓存前，先与服务器验证确认，是否发生变化，没有则可以使用缓存；
- **Cache-Control: must-revalidate，** 缓存不过期就可以用，过期就必须去服务器验证是否还可用。

#### private 和 public
　　上面 4 个字段即可以约束客户端，也可以约束代理。但客户端和代理是不一样的，客户端的缓存只是用户自己使用，而代理的缓存可能会为非常多的客户端提供服务。<br />
　　所以，需要对它的缓存再多一些限制条件。private 和 public 用于区分客户端上的缓存和代理上的缓存。

- **Cache-Control: private，只有客户端能保存缓存，** 是用户私有的，不能放在代理上与别人共享。比如登录论坛，返回的响应报文里用 "Set-Cookie" 添加了论坛 ID，属于私人数据，不能存在代理中；
- **Cache-Control: public，客户端和代理都可以保存缓存，** 完全开放，谁都可以用。

#### 服务端的缓存

- **Cache-Control: proxy-revalidate，** 缓存失效后的重新验证也要区分开（即使用条件请求 "Lastmodified" 和 "ETag"），"must-revalidate" 是只要过期就必须回源服务器验证。"proxy-revalidate" 只要求代理的缓存过期后必须验证，客户端不必回源，只验证到代理这个环节就行了；
- **Cache-Control: s-maxage，缓存的生存时间，** 限定在代理上能够存多久，而客户端仍然使用 "max-age"。比如 Cache-Control: public, max-age=10, s-maxage=30，数据可在浏览器存 10 秒，代理服务器存 30 秒；
- **Cache-Control: no-transform，** 代理专用的属性。代理有时会对缓存数据做优化，比如把图片生成 png、webp 等几种格式，方便今后的请求处理，no-transform 则表示禁止这样的操作；

　　下图为完整的服务端缓存控制策略，可同时控制客户端和代理。

![avatar](photo_3.png)

　　注意，源服务器在设置完 "Cache-Control" 后要为报文加上 "Lastmodified" 或 "ETag" 字段。否则，客户端和代理后面就无法使用条件请求来验证缓存是否有效，也就不会有 304 缓存重定向。

#### 客户端的缓存控制
　　客户端在 HTTP 缓存体系里要面对的是代理和源服务器，也必须区别对待，如下图。

![avatar](photo_4.png)

　　关于缓存的生存时间，多了两个新属性 "max-stale" 和 "min-fresh"。

- **Cache-Control: max-stale，** 如果代理上的缓存过期也可以接受，但不能过期太多，超过 x 秒也会不要。比如 Cache-Control: max-age=5，max-stale=2，表示可缓存 5s，max-stale=2 则表示过期 2 秒的请求也可以接受，总共可缓存 7 秒；
- **Cache-Control: min-fresh，** 缓存必须有效，而且必须在 x 秒后依然有效；
- **Cache-Control: only-if-cached，** 表示只接受代理缓存的数据，不接受源服务器的响应。如果代理上没有缓存或者缓存过期，就应该给客户端返回一个504（Gateway Timeout）。
