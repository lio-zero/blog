# Node.js HTTP 和 HTTPS 模块

[http 模块](https://nodejs.org/api/http.html#http)提供了一种让 Node.js 通过 HTTP（超文本传输协议）传输数据的方法。而 [https 模块](https://nodejs.org/api/https.html#https) 通过 HTTP TLS/SSL 协议传输数据的方法，该协议是安全的 HTTP 协议。

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

## 属性

- `http.METHODS` 属性列出了所有支持的 HTTP 方法：

```bash
> require('http').METHODS
[ 'ACL',
  'BIND',
  'CHECKOUT',
  'CONNECT',
  'COPY',
  'DELETE',
  'GET',
  'HEAD',
  'LINK',
  'LOCK',
  'M-SEARCH',
  'MERGE',
  'MKACTIVITY',
  'MKCALENDAR',
  'MKCOL',
  'MOVE',
  'NOTIFY',
  'OPTIONS',
  'PATCH',
  'POST',
  'PROPFIND',
  'PROPPATCH',
  'PURGE',
  'PUT',
  'REBIND',
  'REPORT',
  'SEARCH',
  'SUBSCRIBE',
  'TRACE',
  'UNBIND',
  'UNLINK',
  'UNLOCK',
  'UNSUBSCRIBE'
]
```

- `http.STATUS_CODES` 属性列出所有 HTTP 状态代码及其描述：

```bash
> require('http').STATUS_CODES
{
  '100': 'Continue',
  '101': 'Switching Protocols',
  '102': 'Processing',
  '103': 'Early Hints',
  '200': 'OK',
  '201': 'Created',
  '202': 'Accepted',
  '203': 'Non-Authoritative Information',
  '204': 'No Content',
  '205': 'Reset Content',
  '206': 'Partial Content',
  '207': 'Multi-Status',
  '208': 'Already Reported',
  '226': 'IM Used',
  '300': 'Multiple Choices',
  '301': 'Moved Permanently',
  '302': 'Found',
  '303': 'See Other',
  '304': 'Not Modified',
  '305': 'Use Proxy',
  '307': 'Temporary Redirect',
  '308': 'Permanent Redirect',
  '400': 'Bad Request',
  '401': 'Unauthorized',
  '402': 'Payment Required',
  '403': 'Forbidden',
  '404': 'Not Found',
  '405': 'Method Not Allowed',
  '406': 'Not Acceptable',
  '407': 'Proxy Authentication Required',
  '408': 'Request Timeout',
  '409': 'Conflict',
  '410': 'Gone',
  '411': 'Length Required',
  '412': 'Precondition Failed',
  '413': 'Payload Too Large',
  '414': 'URI Too Long',
  '415': 'Unsupported Media Type',
  '416': 'Range Not Satisfiable',
  '417': 'Expectation Failed',
  '418': "I'm a Teapot",
  '421': 'Misdirected Request',
  '422': 'Unprocessable Entity',
  '423': 'Locked',
  '424': 'Failed Dependency',
  '425': 'Too Early',
  '426': 'Upgrade Required',
  '428': 'Precondition Required',
  '429': 'Too Many Requests',
  '431': 'Request Header Fields Too Large',
  '451': 'Unavailable For Legal Reasons',
  '500': 'Internal Server Error',
  '501': 'Not Implemented',
  '502': 'Bad Gateway',
  '503': 'Service Unavailable',
  '504': 'Gateway Timeout',
  '505': 'HTTP Version Not Supported',
  '506': 'Variant Also Negotiates',
  '507': 'Insufficient Storage',
  '508': 'Loop Detected',
  '509': 'Bandwidth Limit Exceeded',
  '510': 'Not Extended',
  '511': 'Network Authentication Required'
}
```

- `http.globalAgent` 指向代理对象的全局实例，它是 `http.Agent` 类的一个实例。它用于管理 HTTP 客户端的连接持久性和重用，是 Node HTTP 网络的关键组件。

## 方法

- `http.createServer()` 返回该类的新实例 `http.Server`。

```js
const server = http.createServer((req, res) => {
  // 使用此回调处理每个请求
})
```

- `http.request()` 向服务器发出 HTTP 请求，创建 `http.ClientRequest` 类的实例。
- `http.get()` 与 `http.request()` 类似，但自动将 HTTP 方法设置为 GET，并自动调用 `req.end()`。

## 类

HTTP 模块提供了 5 个类：

- `http.Agent`
- `http.ClientRequest`
- `http.Server`
- `http.ServerResponse`
- `http.IncomingMessage`

### http.Agent

Node 创建 `http.Agent` 类的全局实例。它用于管理 HTTP 客户端的连接持久性和重用，是 Node HTTP 网络的关键组件。

该对象确保向服务器发出的每个请求都排队，并且重用单个套接字。

它还维护一个套接字池。这是性能原因的关键。

### http.ClientRequest

`http.ClientRequest` 对象是在调用 `http.request()` 或 `http.get()` 调用时创建的。

当接收到 `response` 响应时，将使用响应调用响应事件，并将 `http.IncomingMessage` 实例作为参数。

响应返回的数据可以通过两种方式读取：

- 你可以调用 `response.read()` 方法
- 在 `response` 事件处理程序中，可以为 `data` 事件设置事件侦听器 ，以便侦听流入的数据。

### http.Server

此类通常在使用 `http.createServer()` 创建新服务器时实例化并返回。

拥有服务器对象后，您就可以访问其方法：

- `close()` 停止服务器接受新连接
- `listen()` 启动 HTTP 服务器并监听连接

### http.ServerResponse

由 `http.Server` 创建，并作为第二个参数传递给它触发的 `request` 事件。

在代码中通常称为 `res`：

```js
const server = http.createServer((req, res) => {
  // res is an http.ServerResponse object
})
```

处理程序中始终调用的方法是 `end()`，它关闭响应，消息完成，服务器可以将其发送给客户端。必须在每个响应上调用它。

这些方法用于与 HTTP headers 交互：

- `getHeaderNames()` 获取已设置的 HTTP headers 名称列表
- `getHeaders()` 获取已设置的 HTTP headers 的副本
- `setHeader('headerName', value)` 设置 HTTP headers 值
- `getHeader('headerName')` 获取已设置的 HTTP headers
- `removeHeader('headerName')` 删除已设置的 HTTP headers
- `hasHeader('headerName')` 如果响应设置了该 headers，则返回 `true`
- `headersSent()` 如果 headers 已经发送到客户端，则返回 `true`

处理完 headers 后，可以通过调用 `response.writeHead()` 将它们发送到客户端，它接受 `statusCode` 作为第一个参数、可选的状态消息和 headers 对象。

要在响应体中向客户端发送数据，请使用 `write()`。它将缓冲数据发送到 HTTP 响应流。

如果尚未使用 `response.writeHead()` 发送 headers，则它将首先发送 headers，其中包含在请求中设置的状态代码和消息，您可以通过设置 `statusCode` 和 `statusMessage` 属性值来编辑它们：

```js
response.statusCode = 500
response.statusMessage = 'Internal Server Error'
```

### http.IncomingMessage

`http.IncomingMessage` 对象由以下方式创建：

- `http.Server` 侦听 `request` 事件
- `http.ClientRequest` 侦听 `response` 事件

它可用于访问响应：

- status 使用其 `statusCode` 和 `statusMessage` 方法
- headers 使用其 `headers` 方法或 `rawHeaders`
- HTTP 方法使用它的 `method` 方法
- HTTP 版本使用 `httpVersion` 方法
- URL 使用 `url` 方法
- 底层 socket 使用 `socket` 方法

由于 `http.IncomingMessage` 实现了可读的流接口，因此使用流访问数据。
