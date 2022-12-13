# Web Streams

[Streams API](https://developer.mozilla.org/en-US/docs/Web/API/Streams_API) 允许 JavaScript 以编程方式访问从网络接收的数据流，并且允许开发人员根据需要处理它们。

使用流，我们可以从网络或其他来源接收资源，并在第一个 bit 到达时立即处理它，而不用等待资源完全下载后再使用。

## 什么是流（Streams）?

我可能最先想到的一个示例是加载 b 站视频，你不必完全加载它就可以开始观看。

或者直播，你甚至不知道内容什么时候结束。

内容甚至不必结束，它可以无限地产生。

## Streams API

Streams API 允许我们处理此类内容。

我们有 2 种不同的流模式：读取流和写入流。

让我们从可读流开始。

## 可读流

对于可读流，我们有 3 类对象：

- [`ReadableStream`](https://developer.mozilla.org/en-US/docs/Web/API/ReadableStream)
- [`ReadableStreamDefaultReader`](https://developer.mozilla.org/en-US/docs/Web/API/ReadableStreamDefaultReader)
- [`ReadableStreamDefaultController`](https://developer.mozilla.org/en-US/docs/Web/API/ReadableStreamDefaultController)

我们可以使用 ReadableStream 对象来使用流。

下面是可读流的第一个示例。Fetch API 允许从 web 中获取资源，并将其作为流提供：

```js
const stream = fetch('https://github.com/').then((res) => res.body)
```

![ReadableStream 对象](https://upload-images.jianshu.io/upload_images/18281896-1bbb559dd1d4cd4d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

获取响应的 `body` 属性是一个 ReadableStream 对象实例。这是我们的可读流。

## 读取器

对 ReadableStream 对象上调用 `getReader()` 将返回一个 ReadableStreamDefaultReader 对象，即**读取器**。我们可以这样做：

```js
const reader = fetch('https://github.com/').then((res) => res.body.getReader())
```

![ReadableStreamDefaultReader 对象](https://upload-images.jianshu.io/upload_images/18281896-2a359a502b28d731.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们以块的形式读取数据，其中块是一个字节或[类型化数组](https://github.com/lio-zero/blog/blob/main/JavaScript/%E7%B1%BB%E5%9E%8B%E5%8C%96%E6%95%B0%E7%BB%84.md)。数据块在流中排队，我们每次读取一个数据块。

单个流可以包含不同类型的块。

一旦有了 ReadableStreamDefaultReader 对象，就可以使用 `read()` 方法访问数据。

一旦创建了读取器，流就会被锁定，没有其他读取器可以从中获取数据块，直到我们对它调用 `releaseLock()`。

> 你可以 tee 一个流来达到这种效果，稍后将对此进行更多介绍。

## 从可读流中读取数据

一旦我们有了 ReadableStreamDefaultReader 对象实例，我们就可以从中读取数据。

以下是如何从 GitHub 网页逐字节读取 HTML 内容流的第一个块（出于 CORS 原因，你可以在该网页上打开的 DevTools 窗口中执行此操作）。

```js
fetch('https://github.com/').then((res) => {
  res.body
    .getReader()
    .read()
    .then(({ value, done }) => {
      console.log(value)
    })
})
```

![Uint8Array](https://upload-images.jianshu.io/upload_images/18281896-8166b3ea8e6c1ebc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

如果打开每一组数组项，你将看到单个项。这些是存储在 Uint8Array 中的字节：

![字节存储在 Uint8Array 中](https://upload-images.jianshu.io/upload_images/18281896-9ef5b0a28aa95947.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

你可以使用 [Encoding API](https://developer.mozilla.org/en-US/docs/Web/API/Encoding_API) 将这些字节转换为字符：

```js
const decoder = new TextDecoder('utf-8')

fetch('https://github.com/').then((res) => {
  res.body
    .getReader()
    .read()
    .then(({ value, done }) => {
      console.log(decoder.decode(value))
    })
})
```

这将打印出页面中加载的字符：

![将字节转换为字符](https://upload-images.jianshu.io/upload_images/18281896-4276d0f897ac7129.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

以下代码加载流的每个块，并打印出来：

```js
;(async () => {
  const fetchedResource = await fetch('https://github.com/')
  const reader = await fetchedResource.body.getReader()

  let charsReceived = 0
  let result = ''

  reader.read().then(function processText({ done, value }) {
    if (done) {
      console.log('流已完成。收到的内容: ')
      console.log(result)
      return
    }

    console.log(`目前已收到 ${result.length} 个字符!`)

    result += value

    return reader.read().then(processText)
  })
})()
```

我将其包装在一个 `async` 立即执行函数中，以使用 `await`。

> 如果你在浏览器的控制台中测试，无需包括立即执行函数，因为浏览器会优先支持 ES 提案中新出的特性，ES 13 中已支持顶层 `await` 的使用。

我们创建的 `processText()` 函数接收一个具有 2 个属性的对象。

- `done` — 如果流结束并且我们获得了所有数据，则为 `true`
- `value` — 接收到的当前块的值

我们创建这个递归函数来处理整个流。

## 创建流

> WARN：在 Edge 和 IE 中不支持

我们刚刚看到了如何使用 Fetch API 生成的可读流，这是一种开始处理流的好方法，因为用例很实用。

现在让我们看看如何创建一个可读流，这样我们就可以使用代码访问资源。

我们之前已经使用过 ReadableStream 对象。现在，让我们使用 `new` 关键字创建一个全新的：

```js
const stream = new ReadableStream()
```

这个流现在不是很有用。它是一个空流，如果有人想从中读取数据，则没有数据。

我们可以通过在初始化期间传递一个对象来定义流的行为。此对象可以定义这些属性：

- `start` — 创建可读流时调用的函数。在这里，你可以连接到数据源并执行管理任务。
- `pull` — 在未达到内部队列高水位线时，重复调用函数以获取数据。
- `cancel` — 取消流时调用的函数。例如：在接收端调用 `cancel()` 方法时。

下面是对象结构的基本示例：

```js
const stream = new ReadableStream({
  start(controller) {},
  pull(controller) {},
  cancel(reason) {}
})
```

`start()` 和 `pull()` 获得一个控制器对象，它是 ReadableStreamDefaultController 对象的一个实例，允许你控制流状态和内部队列。

为了向流中添加数据，我们调用 `controller.enqueue()` 传递保存数据的变量：

```js
const stream = new ReadableStream({
  start(controller) {
    controller.enqueue('Hello')
  }
})
```

当我们准备关闭流时，我们调用 `controller.close()`。

`cancel()` 获取一个原因，它是在流被取消时提供给 `ReadableStream.cancel()` 方法调用的字符串。

我们还可以传递决定排队策略的可选第二个对象。它包含两个属性：

- `highWaterMark` — 可存储在内部队列中的区块总数。我们在前面讨论 `pull()` 时提到过这一点。
- `size` — 一种可以用来更改块大小的方法，以字节表示。

```js
{
  highWaterMark, size()
}
```

这些主要用于控制流上的压力，尤其是在管道链的上下文中，这在 Web API 中仍处于试验阶段。

当达到流的 highWaterMark 值时，会向管道中的前一个流发送一个背压信号，告诉它们减缓数据压力。

我们有 2 个定义排队策略的内置对象：

- `ByteLengthQueuingStrategy` — 等待块的字节累积大小超过指定的高水位线
- `CountQueuingStrategy` — 会一直等到累积的块数超过指定的高水位线

设置一个 32 字节高水位线的示例：

```js
new ByteLengthQueuingStrategy({ highWaterMark: 32 * 1024 }
```

设置 1 块高水位线的示例：

```js
new CountQueuingStrategy({ highWaterMark: 1 })
```

我提到这一点是为了告诉你，你可以控制流入流中的数据量，并与其他参与者进行通信，但随着事情变得非常复杂，我们不会深入讨论更多细节。

## 准备流

之前我提到过，一旦我们开始读一个取流，它就会被锁定，其他读取器无法访问它，直到我们在它上调用 `releaseLock()`。

但是，我们可以在流本身上使用 `tee()` 方法复制流：

```js
const stream = // ...
const tees = stream.tee()
```

`tees` 现在是一个包含 2 个新流的数组，你可以使用 `tees[0]` 和 `tees[1]` 读取这些流。

## 可写流

对于可写流，我们有 3 类对象：

- [WritableStream](https://developer.mozilla.org/en-US/docs/Web/API/WritableStream)
- [WritableStreamDefaultWriter](https://developer.mozilla.org/en-US/docs/Web/API/WritableStreamDefaultWriter)
- [WritableStreamDefaultController](https://developer.mozilla.org/en-US/docs/Web/API/WritableStreamDefaultController)

我们可以使用 `WritableStream` 对象创建稍后可以使用的流。

创建一个新的可写流：

```js
const stream = new WritableStream()
```

我们必须传递一个对象才能有用。该对象将具有以下可选方法实现：

- `start()` 对象初始化时调用
- `write()` 当块准备好写入接收器时调用（在写入之前保存流数据的底层结构）
- `close()` 完成写入块时调用
- `abort()` 要发出错误信号时调用

大致结构：

```js
const stream = new WritableStream({
  start(controller) {},
  write(chunk, controller) {},
  close(controller) {},
  abort(reason) {}
})
```

`start()`、`close()` 和 `write()` 被传递给控制器，这是一个 WritableStreamDefaultController 对象实例。

至于 `ReadableStream()`，我们可以将第二个对象传递给设置队列策略的 WritableStream。

例如，让我们创建一个流，给定一个存储在内存中的字符串，创建一个消费者可以连接到的流。

我们首先定义一个解码器，使用 Encoding API `TextDecoder()` 构造函数将接收到的字节转换为字符：

```js
const decoder = new TextDecoder('utf-8')
```

我们可以初始化 WritableStream 实现 `close()` 方法，当消息完全接收并且客户端代码调用它时，它将打印到控制台：

```js
const writableStream = new WritableStream({
  write(chunk) {
    // ...
  },
  close() {
    console.log(`信息是 ${result}`)
  }
})
```

我们通过初始化 ArrayBuffer 并将其添加到块来启动 `write()` 实现。然后，我们使用 Encoding API 的 `decoder.decode()` 方法将这个块（一个字节）解码为字符。然后将此值添加到对象外部声明的 `result` 字符串中：

```js
let result

const writableStream = new WritableStream({
  write(chunk) {
    const buffer = new ArrayBuffer(2)
    const view = new Uint16Array(buffer)
    view[0] = chunk
    const decoded = decoder.decode(view, { stream: true })
    result += decoded
  },
  close() {
    // ...
  }
})
```

WritableStream 对象现在已初始化。

我们现在开始实现将使用此流的客户端代码。

我们首先从 `writableStream` 对象获取 `WritableStreamDefaultWriter` 对象：

```js
const writer = writableStream.getWriter()
```

接下来，我们定义要发送的消息：

```js
const message = 'Hello!'
```

然后初始化编码器，对要发送到流的字符进行编码：

```js
const encoder = new TextEncoder()
const encoded = encoder.encode(message, { stream: true })
```

此时，字符串已编码为字节数组。现在，我们在此数组上使用 `forEach` 循环将每个字节发送到流。在每次调用流写入器的 `write()` 方法之前，我们都会检查 `ready` 属性，该属性返回一个 promise，因此我们只在流写入器准备就绪时进行写入：

```js
encoded.forEach((chunk) => writer.ready.then(() => writer.write(chunk)))
```

我们现在唯一错过的是关闭 `writer`。`forEach` 是一个同步循环，这意味着我们只有在每个项目都被写入之后才会到达这一点。

我们仍然检查 `ready` 属性，然后调用 `close()` 方法：

```js
writer.ready.then(() => {
  writer.close()
})
```

## 更多资料

- [精读《web streams》](https://github.com/ascoders/weekly/blob/master/%E5%89%8D%E6%B2%BF%E6%8A%80%E6%9C%AF/214.%E7%B2%BE%E8%AF%BB%E3%80%8Aweb%20streams%E3%80%8B.md)
- [2016 - the year of web streams](https://jakearchibald.com/2016/streams-ftw/)
