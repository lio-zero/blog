# Web Workers

JavaScript 是单线程的，没有任何东西可以同时并行运行。

这很好，因为我们不需要担心并发编程会出现的一整套问题。

有了这个限制，JavaScript 代码从一开始就必须是高效的，否则用户会有不好的体验。昂贵的操作应该是异步的，以避免阻塞线程。

随着 JavaScript 应用程序需求的增长，这在某些场景中开始成为一个问题。

[Web Workers](https://developer.mozilla.org/zh-CN/docs/Web/API/Web_Workers_API) 引入了在浏览器内部**并行**执行的可能性。

通过将耗时的的任务分发给 Web Workers，它将在后台独立运行的 JavaScript，而不会影响页面的性能。

但它也存在一些局限性：

- 无法访问 DOM — `window` 对象和 `document` 对象
- 它可以使用消息传递与主 JavaScript 程序进行通信
- 受[浏览器同源策略](https://github.com/lio-zero/blog/blob/main/%E6%B5%8F%E8%A7%88%E5%99%A8/%E6%B5%8F%E8%A7%88%E5%99%A8%E5%90%8C%E6%BA%90%E7%AD%96%E7%95%A5.md)影响，它需要从同一来源（域、端口和协议）加载
- 如果使用文件协议（`file://`）提供页面，它将不起作用

## 浏览器对 Web Workers 的支持

[Web Workers 的支持情況](https://caniuse.com/?search=Web%20Workers)：

![Web Workers 的支持情況](https://upload-images.jianshu.io/upload_images/18281896-ffca3ed4de89d477.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

可以看到，它可以在任何主流浏览器中使用。

你可以使用以下方式检查 Web Workers 支持：

```js
if (typeof Worker !== 'undefined') {}
```

## 创建 Web Workers

你可以通过初始化一个 Worker 对象来创建一个 Web Workers，并从同一个源加载一个 JavaScript 文件：

```js
const worker = new Worker('worker.js')
```

## 与 Web Workers 的通信

与 Web Workers 通信主要方式有两种：

- Web Workers 对象提供的 [postMessage API](https://developer.mozilla.org/en-US/docs/Web/API/Worker/postMessage)
- [Channel Messaging API](https://developer.mozilla.org/en-US/docs/Web/API/Channel_Messaging_API)

### Web Workers 对象中使用 `postMessage`

你可以在 `Worker` 对象上使用 `postMessage` 发送消息。

> **Tips**：消息是传输的，而不是共享的。

```js
// main.js
const worker = new Worker('worker.js')
worker.postMessage('hello')
```

并使用 `onmessage` 监听消息：

```js
// worker.js
onmessage = (event) => {
  console.log(event.data)
}

onerror = (event) => {
  console.error(event.message)
}
```

`onerror` 监听错误消息。

#### 发回消息

worker 线程可以将消息发送回创建它的函数。使用它的全局 `postMessage()` 方法：

```js
// worker.js
onmessage = (event) => {
  console.log(event.data)
  postMessage('hey')
}

onerror = (event) => {
  console.error(event.message)
}
```

```js
// main.js
const worker = new Worker('worker.js')
worker.postMessage('hello')

worker.onmessage = (event) => {
  console.log(event.data)
}
```

#### 多个事件监听器

使用 `onmessage` 创建的事件监听器，后创建的会覆盖已创建的。

我们可以转而使用 `addEventListener` 方法监听事件：

```js
// worker.js
addEventListener('message', (event) => {
  console.log(event.data)
  postMessage('hey')
})

addEventListener('message', (event) => {
  console.log(`我很好奇，我也在听`)
})

addEventListener('error', (event) => {
  console.log(event.message)
})
```

在 Worker 线程中，`self` 和 `this` 都代表子线程的全局对象。当然，它可以省略，例如上面调用 `addEventListener` 省略了它们。

```js
// main.js
const worker = new Worker('worker.js')
worker.postMessage('hello')

worker.addEventListener('message', (event) => {
  console.log(event.data)
})
```

### 使用 Channel Messaging API

我们可以选择使用更通用的 [Channel Messaging API](https://github.com/lio-zero/blog/blob/main/Web%20API/Channel%20Messaging%20API.md) 来与 Web Workers 通信，而不是使用 Web Workers 提供的内置 postMessage API。

```js
// main.js
const worker = new Worker('worker.js')
const messageChannel = new MessageChannel()

messageChannel.port1.addEventListener('message', (event) => {
  console.log(event.data)
})

worker.postMessage('main', [messageChannel.port2])
```

```js
// worker.js
addEventListener('message', (event) => {
  console.log(event.data)
})
```

Web Workers 可以通过向 `messageChannel.port2` 发送消息来返回消息，如下所示：

```js
addEventListener('message', (event) => {
  event.ports[0].postMessage(data)
})
```

## Web Workers 生命周期

Web Workers 被启动，如果它们没有通过 `worker.onmessage` 或 `addEventListener` 保持在监听模式下，它们将在其代码运行完成后立即关闭。

可以在主线程中使用 `terminate()` 方法停止 Web Workers 运行：

```js
// main.js
const worker = new Worker('worker.js')
worker.postMessage('hello')
worker.terminate()
```

也可以在 Worker 线程内部使用 `close()` 全局方法进行关闭：

```js
// worker.js
onmessage = (event) => {
  console.log(event.data)
  close()
}

onerror = (event) => {
  console.error(event.message)
}
```

## 在 Web Workers 中加载脚本和库

Web Workers 可以使用在其全局作用域中定义的 [`importScripts()`](https://developer.mozilla.org/zh-CN/docs/Web/API/WorkerGlobalScope/importScripts) 方法加载脚本和库：

```js
importScripts('bar.js', 'foo.js')
```

## Web Workers 中可用的 API

如前所述，Web Workers 无法访问 DOM，因此你无法与 `window` 和 `document` 对象进行交互。此外，`parent` 也不可用。

但是，你可以使用许多其他 API，包括：

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
- Console API（`console.log()` 等）
- `setTimeout` 和 `setInterval`
- `addEventListener()` 和 `removeEventListener()`
- Location API
- Navigator API
- WebSockets
- WebGL
- SVG Animations

## 使用库减少工作量

一些库可以帮助我们减少使用 Web Workers 的工作量：

- [comlink](https://github.com/GoogleChromeLabs/comlink) — 基于 Web Workers API 的友好抽象层
- [workerize](https://github.com/developit/workerize) — 在 Web Workers 中运行模块
- [Greenlet](https://github.com/developit/greenlet) — 在 Web Workers 中运行任意一段异步代码

## 更多资料

- [Web Worker 使用教程](https://www.ruanyifeng.com/blog/2018/07/web-worker.html)
- [精读《谈谈 Web Workers》](https://github.com/ascoders/weekly/blob/master/%E5%89%8D%E6%B2%BF%E6%8A%80%E6%9C%AF/76.%E7%B2%BE%E8%AF%BB%E3%80%8A%E8%B0%88%E8%B0%88%20Web%20Workers%E3%80%8B.md)
- [Using Web Workers](https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API/Using_web_workers)
- [7 Things You Need To Know About Web Workers](https://www.developer.com/microsoft/7-things-you-need-to-know-about-web-workers/)
- [When should you be using Web Workers?](https://dassur.ma/things/when-workers/) — 你应该始终使用 Web Workers
- [For the Sake of Your Event Listeners, Use Web Workers](https://macarthur.me/posts/use-web-workers-for-your-event-listeners)
- [The State Of Web Workers In 2021](https://www.smashingmagazine.com/2021/06/web-workers-2021/)
- [Web workers vs Service workers vs Worklets](https://bitsofco.de/web-workers-vs-service-workers-vs-worklets/)
- [Using Web Workers to Speed-Up Your JavaScript Applications](https://blog.teamtreehouse.com/using-web-workers-to-speed-up-your-javascript-applications)
