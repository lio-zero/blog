# HTTP/2

HTTP/2 在 2015 年由 IETF（互联网工程任务组）委员会作为 RFC 7540 发布。它旨在通过提供更有效的互联网传输数据方式来提高原始 HTTP 协议的性能。

**HTTP/2 的性能远远高于 HTTP/1.1。**

HTTP/2 提供的速度缓冲非常吸引人，因此很快就被采用了，只需对 web 服务器进行一个简单的更改（因为 HTTP/2 与 HTTP/1.1 完全向后兼容），您的网站和 web 应用将运行得更快，这反过来对您的用户和 SEO 都是有益的（因为速度是排名的关键因素）。

## HTTP/2 如何比 HTTP/1.1 快得多？

原因有很多，都是为了降低以前版本的低效率，并引入可以让浏览器更快地提供资源的功能。

新版协议的主要特点：

- 请求和响应的多路复用 — 通过单个连接的多个异步 HTTP 请求
- header 压缩 — 新的压缩算法 HPACK 压缩 header
- 服务器推送 — 单个请求的多个响应
- 二进制通信 — 文本通信替换为二进制通信
- 请求优先级 — 先获取重要的数据
- 提高安全性

## 多路复用

在 HTTP/2 之前，每个 TCP 连接一次只能提供一个响应。

TCP 是构建 HTTP 的底层协议。TCP 位于传输层，而 HTTP 位于应用层。

HTTP/2 在单个 TCP 连接上启用了请求/响应多路复用，允许服务器使用同一连接服务多个请求，从而实现更快的通信。

这是对应用有很大好处的变化，并且使一些优化技术过时，包括[精灵图](https://github.com/lio-zero/blog/blob/main/CSS/CSS%20Reset%20%E4%B8%8E%20Sprites.md#%E4%BB%80%E4%B9%88%E6%98%AF%E7%B2%BE%E7%81%B5%E5%9B%BEcss-sprites%E5%85%B6%E4%BC%98%E7%BC%BA%E7%82%B9%E4%BB%A5%E5%8F%8A%E5%A6%82%E4%BD%95%E5%AE%9E%E7%8E%B0)（用于在单个图像中组合多个图像，然后使用特殊的 CSS 技术“解复用”）和域切分，这是另一种用于防止浏览器限制同时连接到同一域的数量的 Hack。

## header 压缩

考虑到 Cookie 和其他 header 值的正常使用，页面和资源上的 HTTP headers 可能会变得非常大。新的 header 压缩算法减小了 headers 大小以提高效率，从而减少了客户端和服务器之间交换的数据量。

## 服务器推送

服务器推送是一项允许对单个请求发送多个响应的功能。由于服务器知道，当请求资源时，客户端将请求其他补充资源（比如 CSS、JS、链接到页面的图像），因此服务器可以决定立即发送它们。

服务器可以完全推送它们，而不是发送 HTML，等待浏览器解析它并触发其他请求来获取资源。

服务器还可以决定发送未来请求中可能需要的资源，预先优化下一页负载并将其放入客户端缓存中。

但需要注意，服务器推送也有其自身的缺点。例如，您可能会向客户端发送太多可能不需要的数据（可能在客户端上已缓存），因此请谨慎使用。

## 二进制通信

HTTP/1.1 使用基于文本的通信。HTTP/2 使用二进制通信，它有一些优点，包括更高效的解析、更少的错误倾向和更紧凑的结构。

## 更多资料

- [Journey to HTTP/2](https://kamranahmed.info/blog/2016/08/13/http-in-depth)
- [解密 HTTP/2 与 HTTP/3 的新特性](https://juejin.cn/post/6844903968380813325)
- [Getting Ready For HTTP2: A Guide For Web Designers And Developers](https://www.smashingmagazine.com/2016/02/getting-ready-for-http2/)
- [A Comprehensive Guide To HTTP/2 Server Push](https://www.smashingmagazine.com/2017/04/guide-http2-server-push/)
- [Will WebSocket survive HTTP/2?](https://www.infoq.com/articles/websocket-and-http2-coexist/)
- [Using SSE Instead Of WebSockets For Unidirectional Data Flow Over HTTP/2](https://www.smashingmagazine.com/2018/02/sse-websockets-data-flow-http2/)
- [简单讲解一下 http2 的多路复用](https://github.com/Advanced-Frontend/Daily-Interview-Question/issues/14)
- [What is HTTP/2 – The Ultimate Guide](https://kinsta.com/learn/what-is-http2/)
- [http2-explained](https://http2-explained.haxx.se/zh)
