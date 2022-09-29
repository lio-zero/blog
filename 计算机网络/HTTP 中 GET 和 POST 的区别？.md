# HTTP 中 GET 和 POST 的区别？

从原理性看：

- 根据 HTTP 规范，GET 请求用于获取信息，而且应该是安全和幂等的，而 POST 请求表示可能修改服务器上资源

从表面上看：

- GET 请求的数据会附在 URL 后面（浏览器地址栏、Req Header 上），POST 的数据放在 HTTP 包体（Req Body 上）
- POST 安全性比 GET 安全性高，因为参数直接暴露在 URL 上，所以不能用来传递敏感信息。

> 这里的安全性是有待思考的，可以在 [Is either GET or POST more secure than the other?](https://stackoverflow.com/questions/198462/is-either-get-or-post-more-secure-than-the-other) 查看讨论结果。

下面是一些具体的区别。

GET 请求：

- GET 请求可以被缓存
- GET 请求保留在浏览器历史记录中
- GET 请求可以添加书签
- 处理敏感数据时，绝不应使用 GET 请求
- GET 请求有长度限制
- GET 请求应仅用于检索数据
- GET 产生一个 TCP 数据包
- GET 请求仅支持进行 URL 编码
- GET 参数通过 URL 传递

POST 请求：

- POST 请求永远不会被缓存
- POST 请求不会保留在浏览器历史记录中
- 不能为 POST 请求添加书签
- POST 请求对数据长度没有限制
- POST 产生两个 TCP 数据包
- POST 支持多种编码方式
- POST 放在请求体（Request body）中

需要注意的是，GET 请求提交的数据大小长度并没有限制，HTTP 协议规范没有对 URL 长度进行限制。这里的 GET 长度有限制，是指特定的浏览器及服务器对它的限制。

不同的浏览器长度限制可以查阅 [What is the maximum length of a URL in different browsers?](https://stackoverflow.com/questions/417142/what-is-the-maximum-length-of-a-url-in-different-browsers?rq=1)。服务器的话可以查阅 [Is there a practical HTTP Header length limit?](https://stackoverflow.com/questions/1097651/is-there-a-practical-http-header-length-limit)。

也可以看看 [99% 的人都理解错了 HTTP 中 GET 与 POST 的区别](https://mp.weixin.qq.com/s?__biz=MzI3NzIzMzg3Mw==&mid=100000054&idx=1&sn=71f6c214f3833d9ca20b9f7dcd9d33e4#rd)。
