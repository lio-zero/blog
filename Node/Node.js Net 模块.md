# Node.js Net 模块

[`net` 模块](https://nodejs.org/api/net.html#net)提供了一种创建 TCP 服务器和 TCP 客户端的方法。它是 Node.js 通讯功能实现的基础。

`net` 模块主要包含两部分：

- [`net.Server`](https://nodejs.org/api/net.html#class-netserver) 用于创建 TCP 或 IPC 服务器。
- [`net.Socket`](https://nodejs.org/api/net.html#class-netsocket) 对象是 TCP 或 UNIX Socket 的抽象。`net.Socket` 实例实现了一个双工流接口，可以在用户创建客户端（使用 `connect()`）时使用，或者由 Node 创建它们，并通过 `connection` 服务器事件传递给用户。

它们各自都实现了很多属性和方法，详细查阅文档。

我们在建立服务器时会使用到的 `http.createServer`，它是在 `net.createServer` 方法的基础上建立的。以下是一个简单的示例：

```js
// server.js
const net = require('net')
const port = 8080
const server = net.createServer((connection) => {
  console.log('client connected')
  connection.on('end', () => {
    console.log('客户端关闭连接')
  })
  connection.write('Hello Node.js!')
  connection.pipe(connection)
})

server.listen(port, () => {
  console.log(`App listening on port ${port}`)
})
```

`client.js` 文件：

```js
const net = require('net')

const client = net.connect({ port: 8080 }, () => {
  console.log('连接到服务器！')
})

client.on('data', (data) => {
  console.log(data.toString())
  client.end()
})

client.on('end', () => {
  console.log('断开与服务器的连接')
})
```
