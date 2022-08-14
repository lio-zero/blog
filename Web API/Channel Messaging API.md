# Channel Messaging API

给定在同一文档中但在不同上下文中运行的两个脚本，[Channel Messaging API](https://developer.mozilla.org/en-US/docs/Web/API/Channel_Messaging_API) 允许它们通过通道传递消息进行通信。

[Channel Messaging API 支持情况](https://caniuse.com/?search=Channel%20Messaging%20)：

![Channel Messaging API 支持情况](https://upload-images.jianshu.io/upload_images/18281896-e28f6b742bc95a17.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 基本用法

调用 `new MessageChannel()` 消息通道被初始化。

```js
const channel = new MessageChannel()
```

通道有 2 个属性，称为：

- `port1`
- `port2`

这些属性是 [`MessagePort`](https://developer.mozilla.org/en-US/docs/Web/API/MessagePort) 对象。`port1` 是创建通道的部分使用的端口，`port2` 是通道接收器使用的端口（顺便说一下，通道是双向的，因此接收器也可以发回消息）。

在通道的每一端，您在一个端口上监听，并将消息发送到另一个端口。

发送消息通过 `postMessage` 方法：

```js
otherWindow.postMessage()
```

`otherWindow` 是其他浏览上下文。它接受消息、来源和端口。例如：

```js
const data = { name: 'O.O' }
const channel = new MessageChannel()
window.postMessage(data, [channel.port2])
```

消息可以是任何受支持的值：

- 所有基本类型，但不包括 `symbol`
- 数组
- 对象字面量
- String、Date 和 RegExp 对象
- Blob、File 和 FileList 对象
- ArrayBuffer 和 ArrayBufferView 对象
- FormData 对象
- ImageData 对象
- Map 和 Set 对象

Origin（源）是一个 URI（例如 `https://example.com`）。您可以使用 `*` 来允许不太严格的检查，或指定域，或指定 `/` 来设置同域目标，而无需指定它是哪个域。

另一个浏览上下文使用 `message` 事件监听消息：

```js
self.addEventListener('message', (event) => {
  console.log('新消息到达!')
})
```

在这种情况下，`self` 与使用 `window` 相同，你也可以省略。

在事件处理程序中，我们可以通过查看事件对象的 `data` 属性来访问发送的数据：

```js
self.addEventListener('message', (event) => {
  console.log('新消息到达!')
  console.log(event.data)
})
```

我们可以使用 `MessagePort.postMessage` 进行回复：

```js
self.addEventListener('message', (event) => {
  console.log('新消息到达!')
  console.log(event.data)

  const data = { message: 'hey' }
  event.ports[0].postMessage(data)
})
```

可以通过调用端口上的 `close()` 方法关闭通道：

```js
self.addEventListener('message', (event) => {
  console.log('新消息到达!')
  console.log(event.data)

  const data = { message: 'hey' }
  event.ports[0].postMessage(data)
  event.ports[0].close()
})
```

## 带有 iframe 的示例

下面是一个在文档和嵌入其中的 `iframe` 之间进行通信的示例。

主文档定义了一个 `iframe` 和 `span`，我们将在其中打印从 `iframe` 文档发送的消息。加载 `iframe` 文档后，我们会在创建的通道上向其发送一条消息。

```html
<!DOCTYPE html>
<html>
  <body>
    <iframe src="iframe.html" />
    <span></span>
  </body>
  <script>
    const channel = new MessageChannel()
    const display = document.querySelector('span')
    const iframe = document.querySelector('iframe')

    iframe.addEventListener('load', () => {
      iframe.contentWindow.postMessage('Hey', '*', [channel.port2])
    })

    channel.port1.onmessage = (event) => {
      display.innerHTML = event.data
    }
  </script>
</html>
```

`iframe` 页面内容更简单：

```html
<!DOCTYPE html>
<html>
  <script>
    window.addEventListener('message', (event) => {
      // 发回消息
      event.ports[0].postMessage('从 iframe 返回的消息')
    })
  </script>
</html>
```

如您所见，我们甚至不需要初始化通道，因为 `message` 当从容器页面接收到消息时，处理程序会自动运行。

发送的事件由以下属性组成：

- `data` — 从另一个窗口发送的对象
- `origin` — 发送消息的窗口的原始 URI
- `source` — 发送消息的窗口对象

始终验证消息发送人的来源。

`event.ports[0]` 是我们在 `iframe` 中引用 `port2` 的方式，因为 `ports` 是一个数组，并且端口被添加为第一个元素。

## Service Worker 的例子

Service Worker 是一个事件驱动的 Worker，是一个与网页相关联的 JavaScript 文件。

重要的是要知道 Service Worker 与主线程是隔离的，我们必须使用消息与它们进行通信。

下面是附加到主文档中的脚本处理向 Service Worker 发送消息的方式:

```js
// worker 是已实例化的 Service Worker

const messageChannel = new MessageChannel()
messageChannel.port1.addEventListener('message', (event) => {
  console.log(event.data)
})

worker.postMessage(data, [messageChannel.port2])
```

在 Service Worker 代码中，我们为 `message` 事件添加了一个事件监听器：

```js
self.addEventListener('message', (event) => {
  console.log(event.data)
})
```

它可以通过向 `messageChannel.port2` 发送消息来发回消息：

```js
self.addEventListener('message', (event) => {
  event.ports[0].postMessage(data)
})
```
