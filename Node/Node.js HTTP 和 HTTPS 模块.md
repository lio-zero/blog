# Node.js HTTP 和 HTTPS 模块

[http 模块](http://nodejs.cn/api/http.html#http)提供了一种让 Node.js 通过 HTTP（超文本传输协议）传输数据的方法。而 [https 模块](http://nodejs.cn/api/https.html#https) 通过 HTTP TLS/SSL 协议传输数据的方法，该协议是安全的 HTTP 协议。

> 各种 Node HTTP 服务框架的底层原理都是离不开该模块。

使用内置的 `http` 模块，我们可以快速的搭建一个简单的 HTTP 服务器。该服务器允许我们监听任意端口并提供一个回调函数，该函数将在每个传入请求时调用。

回调将接收两个参数：一个 [Request 对象](https://nodejs.org/dist/latest-v6.x/docs/api/http.html#http_class_http_incomingmessage)和一个 [Response 对象](https://nodejs.org/dist/latest-v6.x/docs/api/http.html#http_class_http_serverresponse)。Request 对象将填充有关请求的有用属性，而 Response 对象将用于向客户端发送响应。

示例：

```js
const http = require('http')
const PORT = 5000
const router = (req, res) => {
  console.log(`${new Date().toISOString()} - ${req.method} ${req.url}`)
  res.setHeader('Content-Type', 'text/plain')
  res.end('Hello World!')
}

http.createServer(router).listen(PORT)
```

这将启动一个 Web 服务器，监听端口 5000。当请求进入时，控制台将记录一条消息，其中包含当前日期、请求的方法和 URL。

该数据来自参数 `req`，它是 Node 创建的 Request 对象。此 Request 对象还有一个名为 `socket` 的属性，它是较低级别的 TCP 套接字。

> ...
