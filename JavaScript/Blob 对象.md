# Blob 对象

Web 浏览器实现了一个 [`Blob`](https://developer.mozilla.org/zh-CN/docs/Web/API/Blob) 对象，该对象负责保存数据。

Blob 的意思是“二进制大对象”，它是字节块的不透明表示。

Blob 用于许多事情：

- Blob 可以从网络内容中创建，可以保存到磁盘，也可以从磁盘读取。Blob 是 `FileReader` API 中使用的文件的底层数据结构。
- Blob 可以使用 [Channel Messaging API](https://developer.mozilla.org/en-US/docs/Web/API/Channel_Messaging_API) 在 [Web Worker](https://developer.mozilla.org/zh-CN/docs/Web/API/Web_Workers_API/Using_web_workers) 和 [iFrame](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/iframe) 之间发送，也可以使用 [Push API](https://developer.mozilla.org/en-US/docs/Web/API/Push_API) 从服务器发送到客户端。
- 使用 URL 引用 Blob。
- 您可以提取存储在 Blob 中的文本（或字节），Blob 甚至可以直接存储在 [IndexedDB](https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API) 中，也可以从那里检索它们。

## 基本用法

创建 Blob 的方式：

- 使用 `Blob()` 构造函数
- 从另一个 Blob，使用 `Blob.slice()` 实例方法

构造函数接受一个值数组。即使只有一个字符串要放入 Blob，也必须将其包装在数组中。

语法：

```js
const Blob = new Blob(array, options)
```

例子：

```js
const data = new Blob(['Test'])
```

`array` 可以是：

- 字符串（包括 [DOMString](https://developer.mozilla.org/zh-CN/docs/conflicting/Web/JavaScript/Reference/Global_Objects/String_6fa58bba0570d663099f0ae7ae8883ab)）
- [ArrayBuffer](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/ArrayBuffer)
- [ArrayBufferView](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/TypedArray)
- 另一个 Blob

Blob 构造函数接受一个可选的第二个参数，该参数表示 [MIME 类型](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Basics_of_HTTP/MIME_Types)：

```js
const data = new Blob(['Test'], { type: 'text/plain' })
```

一旦拥有 Blob 对象后，可以访问其 2 个属性：

- `size` 返回 Blob 内容的长度（以字节为单位）
- `type` 与之关联的 MIME 类型
- 你可以调用它唯一的 `slice()` 方法

调用 `slice()` 时，你可以检索 Blob 的一部分。

这是从 `aBlob` 字节 10 到 20 创建新 blob 的示例：

```js
const partialBlob = aBlob.slice(10, 20)
```

## 上传 Blob

此代码用作对输入类型或拖放的回调。我们使用 XHR 将 Blob 加载到 URL，使用 `trackProgress` 函数来跟踪进度：

```js
const uploadBlob = (url, blob, trackProgress) => {
  const xhr = new XMLHttpRequest()
  xhr.open('POST', url)
  xhr.send(blob)
  xhr.upload.onprogress = trackProgress
}
```

## 以 Blob 形式从互联网下载数据

我们可以从互联网下载数据，并使用以下语法将其存储到 Blob 对象中：

```js
const downloadBlob = (url, callback) => {
  const xhr = new XMLHttpRequest()
  xhr.open('GET', url)
  xhr.responseType = 'blob'

  xhr.onload = () => {
    callback(xhr.response)
  }

  xhr.send(null)
}
```

## Blob URL

前面说过，Blob 也可以使用 URL 来引用。

Blob URL 由浏览器生成，是内部引用。给定一个 Blob，您可以使用该 URL 生成指向它的 `URL.createObjectURL()` 函数。

Blob URL 以 `blob://` 开头。

一旦浏览器看到该 URL，它将提供存储在内存或磁盘中的相应 Blob。

Blob URL 不同于 Data URL（以 `data:` 开头），因为它们不将数据存储在 URL 中。它也不同于 File URL（以 `file:` 开头），因为它们不会显示文件路径等敏感信息。

如果您访问一个不再存在的 Blob URL，您将收到来自浏览器的 404 错误。

生成 Blob URL 后，可以通过调用 [`URL.revokeObjectURL()`](https://developer.mozilla.org/zh-CN/docs/Web/API/URL/revokeObjectURL) 并传递 URL 来删除它。

## 示例：从页面上的本地磁盘加载文件并获取

在这个示例代码中，我们有一个 `input` 元素来选择图像。一旦选择了图像，我们删除输入元素并显示图像。我们还会在完成图像显示后清除 Blob：

```html
<input type="file" allow="image/*" />
```

```js
const input = document.querySelector('input')

input.addEventListener('change', (e) => {
  const img = document.createElement('img')
  const imageBlob = URL.createObjectURL(input.files[0])
  img.src = imageBlob

  img.onload = function () {
    URL.revokeObjectURL(imageBlob)
  }

  input.parentNode.replaceChild(img, input)
})
```

## 从 Blob 中读取

您不能直接访问 Blob 中包含的数据。

为此，我们必须使用 [`FileReader`](https://developer.mozilla.org/zh-CN/docs/Web/API/FileReader) 对象。

```js
const reader = new FileReader()

reader.addEventListener('load', () => {
  console.log(reader.result) // 'Test'
})

reader.readAsText(data)
```

Blob 中的内容的另一种读取方式是使用响应对象。

```js
const text = await new Response(data).text()
text // 'Test'
```

## 更多资料

- [Blob](https://zh.javascript.info/blob)
- [你不知道的 Blob](https://juejin.cn/post/6844904178725158926)
- [《你不知道的 Blob》番外篇](https://juejin.cn/post/6844904183661854727)
