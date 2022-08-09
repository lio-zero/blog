# WebSocket

[WebSocket](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket) 是 Web 应用程序中 HTTP 通信的替代方案。

它在客户端和服务器之间提供了一个长时间的双向通信通道。

一旦建立，通道将保持开放状态，以低延迟和开销提供非常快速的连接。

以下是 [WebSocket 的支持情况](https://caniuse.com/websockets)：

![浏览器对 WebSocket 的支持](https://upload-images.jianshu.io/upload_images/18281896-60ead383f2d687ee.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

可以看到，所有现代浏览器都支持 WebSocket。

## WebSocket 与 HTTP 的区别

HTTP 是一种非常不同的协议，也是一种不同的通信方式。

HTTP 是一种请求/响应协议：当客户端请求时，服务器会返回一些数据。

使用 WebSocket：

- 服务器可以向客户端发送消息，而无需客户端明确请求
- 客户端和服务器可以同时相互通信
- 发送消息需要交换非常少的数据开销。这意味着低延迟通信。

WebSocket 非常适合实时和长时间的通信。

HTTP 非常适合偶尔进行数据交换和客户端发起的交互。

HTTP 的实现要简单得多，而 WebSocket 需要更多的开销。

## 安全的 WebSocket

始终对 WebSocket 使用安全、加密的协议 `wss://`。

`ws://` 指的是不安全的 WebSocket 版本（`WebSocket` 的 `http:`），出于显而易见的原因，应避免使用。

## 创建新的 WebSocket 连接

```js
const url = 'wss://myServer.com'
const connection = new WebSocket(url)
```

`connection` 是一个 WebSocket 对象。

成功建立连接后，将触发 `open` 事件。

通过将回调函数分配给 `connection` 对象的 `onopen` 属性来监听它：

```js
connection.onopen = () => {
  //...
}
```

如果出现任何错误，将触发 `onerror` 函数回调：

```js
connection.onerror = (error) => {
  console.log(`WebSocket error: ${error}`)
}
```

## 使用 WebSocket 将数据发送到服务器

一旦连接打开，您就可以向服务器发送数据。

您可以在 `onopen` 回调函数中方便地执行此操作：

```js
connection.onopen = () => {
  connection.send('hey')
}
```

## 使用 WebSocket 从服务器接收数据

使用 `onmessage` 上的回调函数进行监听，该函数在收到 `message` 事件时调用：

```js
connection.onmessage = (e) => {
  console.log(e.data)
}
```

## 在 Node.js 中实现服务器

[`ws`](https://github.com/websockets/ws) 是 Node.js 的一个流行 WebSocket 库。

我们将使用它来构建 WebSocket 服务器。它还可以用于实现客户端，并使用 WebSocket 在两个后端服务之间进行通信。

使用 pnpm 安装它：

```js
pnpm i ws
```

您只需要编写的代码很少：

```js
const WebSocket = require('ws')

const wss = new WebSocket.Server({ port: 8080 })

wss.on('connection', (ws) => {
  ws.on('message', (message) => {
    console.log(`Received message => ${message}`)
  })
  ws.send('ho!')
})
```

此代码在端口 8080（WebSocket 的默认端口）上创建一个新服务器，并在建立连接时添加一个回调函数，发送 `ho!` 到客户端，并记录它接收到的消息。

## 调试 websocket

Chrome 和 Firefox 有一种方便的方式来可视化通过 WebSocket 发送的所有信息。他们对 DevTools 的支持一直在提高。

在这两种情况下，您都可以查看 Network 面板，并选择 WS 以仅过滤 WebSocket 连接。

![Chrome 演示](https://upload-images.jianshu.io/upload_images/18281896-ac9d3e9fff5f196b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![Chrome 详细信息](https://upload-images.jianshu.io/upload_images/18281896-763f91c206013c05.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

Firefox DevTools 在这方面要做的更好，能展示的内容更多：

![Firefox 中的数据](https://upload-images.jianshu.io/upload_images/18281896-432939dd4daed395.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

查看这篇关于 [Mozilla Hacks 的帖子](https://hacks.mozilla.org/2019/10/firefoxs-new-websocket-inspector/)，了解更多关于如何使用这个工具的信息。

## 更多资料

[WebSocket 教程](https://www.ruanyifeng.com/blog/2017/05/websocket.html)
