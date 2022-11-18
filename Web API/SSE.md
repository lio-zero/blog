# SSE

[SSE](https://developer.mozilla.org/zh-CN/docs/Web/API/Server-sent_events/Using_server-sent_events)（Server Sent Events，服务器发送事件）是一种新的 HTTP API，用于将事件从服务器推送到客户端。

与 WebSocket 不同，服务器发送的事件建立在 HTTP 协议之上，因此不需要 `ws://` URL 或额外的 npm 模块。

SSE 还会[自动处理重新连接](https://web.dev/eventsource-basics/#toc-reconnection-timeout)，因此，如果连接丢失，无需编写代码重新连接。

在客户端，我们可以使用 [`EventSource`](https://developer.mozilla.org/en-US/docs/Web/API/Server-sent_events#Interfaces) 类连接到服务器端事件源。

下面我们以 Node.js Express 框架作为服务器写一个简单的示例。

在客户端，我们只需将 `EventSource` 类指向配置为处理 SSE 的 Express 路由，并监听 `message` 事件。

```js
const source = new EventSource('/events')

source.addEventListener('message', (event) => {
  console.log('New message: ', event.data)
})
```

Express 方面比较复杂。要支持 SSE，我们需要设置 3 个 header，然后使用 [`flushHeaders()`](https://nodejs.org/api/http.html#http_request_flushheaders) 将 header 发送到客户端：

- `Cache-Control: no-cache` — 不使用资源的缓存版本
- `Content-Type: text/event-stream` — 所有客户端知道这个响应是一个 HTTP stream
- `Connection: keep-alive` — 所有 Node.js 知道保持 HTTP socket 打开

调用 `flushHeaders()` 后，你就可以开始使用 `res.write()` 方法编写事件了。

**为什么使用 `res.write()` 而不使用 `res.send()` 或 `res.end()` 呢？**

`res.write()` 方法写入 HTTP 连接而不显式结束 HTTP 响应。需要确保不使用 `res.send()` 或 `res.end()`，因为它们会显式结束响应。

以下是使用 `/events` 接口处理 SSE 的独立 Express 服务器的示例：

```js
const express = require('express')
const fs = require('fs')

async function run() {
  const app = express()

  app.get('/events', async (req, res) => {
    res.set({
      'Cache-Control': 'no-cache',
      'Content-Type': 'text/event-stream',
      Connection: 'keep-alive'
    })
    res.flushHeaders()

    // 如果连接丢失，告诉客户端每 10 秒重试一次
    res.write('retry: 10000\n\n')
    let count = 0

    while (true) {
      await new Promise((resolve) => setTimeout(resolve, 1000))

      console.log('Emit', ++count)
      // 将包含当前 count 的 SSE 作为字符串发出
      res.write(`data: ${count}\n\n`)
    }
  })

  const index = fs.readFileSync('./index.html', 'utf8')
  app.get('/', (req, res) => res.send(index))

  await app.listen(3000)
  console.log('Listening on port 3000')
}

run().catch(console.err)
```

这里需要注一下，服务器发送每一条消息由 `\n\n` 分割，一条消息有几个可用字段：

- `data` — 消息体（body），一系列多个 `data` 被解释为单个消息，各个部分之间由 `\n` 分隔。
- `id` — 更新 `lastEventId`，重连时以 `Last-Event-ID` 发送此 id。
- `retry` — 建议重连的延迟，以 ms 为单位。无法通过 JavaScript 进行设置。
- `event` — 事件名，必须在 `data:` 之前。

如果存在疑问，可以查看下一节提供的链接，有详细讲解。

运行上面的服务器，并访问 `http://localhost:3000`，打开控制台查看消息。

## 最后

本文介绍了什么是 SSE，以及使用一个 Express 示例，了解如何在客户端和服务端使用 SSE。

关于 SSE 的详细内容，例如 `EventSource` 对象的具体事件/方法、服务器的响应格式等，可以查阅 [Server Sent Events](https://zh.javascript.info/server-sent-events)。

## 更多资料

- [Using SSE Instead Of WebSockets For Unidirectional Data Flow Over HTTP/2](https://www.smashingmagazine.com/2018/02/sse-websockets-data-flow-http2/)
- [Implementing Push Technology Using Server-Sent Events](https://www.sitepoint.com/implementing-push-technology-using-server-sent-events/)
