# Web Workers

JavaScript 是单线程的，没有任何东西可以同时并行运行。

这很好，因为我们不需要担心并发编程会出现的一整套问题。

有了这个限制，JavaScript 代码从一开始就必须是高效的，否则用户会有不好的体验。昂贵的操作应该是异步的，以避免阻塞线程。

随着 JavaScript 应用程序需求的增长，这在某些场景中开始成为一个问题。

[Web Workers](https://developer.mozilla.org/zh-CN/docs/Web/API/Web_Workers_API) 引入了在浏览器内部并行执行的可能性。

但它也存在一些局限性：

- 无法访问 DOM — `window` 对象和 `document` 对象
- 它可以使用消息传递与主 JavaScript 程序进行通信
- 受[浏览器同源策略](https://github.com/lio-zero/blog/blob/main/%E6%B5%8F%E8%A7%88%E5%99%A8/%E6%B5%8F%E8%A7%88%E5%99%A8%E5%90%8C%E6%BA%90%E7%AD%96%E7%95%A5.md)影响，它需要从同一来源（域、端口和协议）加载
- 如果使用文件协议（`file://`）提供页面，它将不起作用

Web Worker 的全局作用域是 [`WorkerGlobalScope`](https://developer.mozilla.org/en-US/docs/Web/API/WorkerGlobalScope) 对象，而不是主线程中的 Window。

## 浏览器对 Web Workers 的支持

[Web Workers 的支持情況](https://caniuse.com/?search=Web%20Workers)：

![Web Workers 的支持情況](https://upload-images.jianshu.io/upload_images/18281896-ffca3ed4de89d477.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

可以看到，它可以在任何主流浏览器中使用。

您可以使用以下方式检查 Web Workers 支持：

```js
if (typeof Worker !== 'undefined') { }
```

## 创建 Web Worker

你可以通过初始化一个 Worker 对象来创建一个 Web Worker，并从同一个源加载一个 JavaScript 文件:

```js
const worker = new Worker('worker.js')
```

## 与 Web Worker 的通信

与 Web Worker 通信主要方式有两种：

- Web Worker 对象提供的 [postMessage API](https://developer.mozilla.org/en-US/docs/Web/API/Worker/postMessage)
- [Channel Messaging API](https://developer.mozilla.org/en-US/docs/Web/API/Channel_Messaging_API)

### Web Worker 对象中使用 postMessage

您可以在 `Worker` 对象上使用 `postMessage` 发送消息。

> 重要提示：消息是传输的，而不是共享的。

`main.js`：

```js
const worker = new Worker('worker.js')
worker.postMessage('hello')
```

`worker.js`：

```js
onmessage = (event) => {
  console.log(event.data)
}

onerror = (event) => {
  console.error(event.message)
}
```

#### 发回消息

工作人员可以将消息发送回创建它的函数。使用它的全局 `postMessage()` 函数：

`worker.js`:

```js
onmessage = (event) => {
  console.log(event.data)
  postMessage('hey')
}

onerror = (event) => {
  console.error(event.message)
}
```

`main.js`:

```js
const worker = new Worker('worker.js')
worker.postMessage('hello')

worker.onmessage = (event) => {
  console.log(event.data)
}
```

#### 多个事件监听器

如果要为 `message` 事件设置多个侦听器，而不是使用 `onmessage` 创建事件侦听器（也适用于 `error` 事件）：

`worker.js`:

```js
addEventListener(
  'message',
  (event) => {
    console.log(event.data)
    postMessage('hey')
  },
  false
)

addEventListener(
  'message',
  (event) => {
    console.log(`我很好奇，我也在听`)
  },
  false
)

addEventListener(
  'error',
  (event) => {
    console.log(event.message)
  },
  false
)
```

在 Worker 线程中，`self` 和 `this` 都代表子线程的全局对象。当然，它可以省略，例如上面调用 `addEventListener` 省略了它们。

`main.js`:

```js
const worker = new Worker('worker.js')
worker.postMessage('hello')

worker.addEventListener(
  'message',
  (event) => {
    console.log(event.data)
  },
  false
)
```

### 使用 Channel Messaging API

我们可以选择使用更通用的 Channel Messaging API 来与 Web Workers 通信，而不是使用 Web Workers 提供的内置 postMessage API。

`main.js`:

```js
const worker = new Worker('worker.js')
const messageChannel = new MessageChannel()

messageChannel.port1.addEventListener('message', (event) => {
  console.log(event.data)
})

worker.postMessage('main', [messageChannel.port2])
```

`worker.js`:

```js
addEventListener('message', (event) => {
  console.log(event.data)
})
```

Web Worker 可以通过向 `messageChannel.port2` 发送消息来返回消息，如下所示：

```js
addEventListener('message', (event) => {
  event.ports[0].postMessage(data)
})
```

## Web Worker 生命周期

Web Worker 被启动，如果它们没有通过 `worker.onmessage` 或添加事件侦听器保持在侦听模式下，它们将在其代码运行完成后立即关闭。

可以在主线程中使用 `terminate()` 方法停止 Web Worker，并在 Worker 内部使用 `close()` 全局方法:

`main.js`:

```js
const worker = new Worker('worker.js')
worker.postMessage('hello')
worker.terminate()
```

`worker.js`:

```js
onmessage = (event) => {
  console.log(event.data)
  close()
}

onerror = (event) => {
  console.error(event.message)
}
```

## 在 Web Worker 中加载库

Web Workers 可以使用在其全局作用域中定义的 [`importScripts()`](https://developer.mozilla.org/zh-CN/docs/Web/API/WorkerGlobalScope/importScripts) 全局函数：

```js
importScripts('../utils/file.js', './something.js')
```

## Web Workers 中可用的 API

如前所述，Web Worker 无法访问 DOM，因此您无法与 `window` 和 `document` 对象进行交互。此外，`parent` 不可用。

但是，您可以使用许多其他 API，包括：

- XHR API
- Fetch API
- BroadcastChannel API
- FileReader API
- IndexedDB
- Notifications API
- Promises
- Service Workers
- Channel Messaging API
- Cache API
- Console API（`console.log()`...）
- JavaScript Timers（`setTimeout`，`setInterval`...）
- CustomEvents API: `addEventListener()` and `removeEventListener()`
- 当前 URL，您可以在读取模式下通过 `location` 属性访问该 URL
- WebSockets
- WebGL
- SVG Animations

## 更多资料

- [Web Worker 使用教程](https://www.ruanyifeng.com/blog/2018/07/web-worker.html)
- [The State Of Web Workers In 2021](https://www.smashingmagazine.com/2021/06/web-workers-2021/)
- [Web workers vs Service workers vs Worklets](https://bitsofco.de/web-workers-vs-service-workers-vs-worklets/)
- [Using Web Workers to Speed-Up Your JavaScript Applications](https://blog.teamtreehouse.com/using-web-workers-to-speed-up-your-javascript-applications)
