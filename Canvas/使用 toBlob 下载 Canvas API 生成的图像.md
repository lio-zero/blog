# 使用 toBlob 下载 Canvas API 生成的图像

我们以一个 SVG 转为 Canvas 为例，使用 `toBlob` 方法下载 Canvas API 生成的图像。

## 内联 SVG 到画布

首先，您需要使用 DOM 方法来选择您的内联 SVG 元素。

假设我们的 SVG 有一个值为 `my-svg` 的 id：

```js
const mySVG = document.getElementById('my-svg')
```

然后，使用 `XMLSerializer` 序列化 SVG 和 btoa（二进制到 ASCII）的内容来创建它的 base64 版本：

```js
const xml = new XMLSerializer().serializeToString(mySVG)
const svg64 = btoa(xml)
```

然后，您可以直接从 base64 编码的字符串生成可下载的文件，但我们面临的问题是大多数浏览器对要作为文件下载的这种 base64 编码的字符串的大小施加了限制。所以，我们不得不从这一点开始努力绕过这个限制。

创建一个新的 HTML `img` 元素并将其 `src` 属性设置为我们的 SVG 的 base64 版本：

```js
const image = new Image()
const b64Start = 'data:image/svg+xml;base64,'
image.src = b64Start + svg64
```

现在我们在页面上有一个带有 SVG 镜像的实际 HTML `img` 元素，我们可以将它绘制到 Canvas 2D 上下文中。

首先，我们需要为我们的图像大小创建一个 Canvas 2D 上下文。假设您在页面上已经有一个 `canvas` 元素，其 id 值为 `my-canvas`：

```js
const canvas = document.getElementById('my-canvas')
const ctx = canvas.getContext('2d')
ctx.fillStyle = 'white'
ctx.fillRect(0, 0, image.naturalWidth, image.naturalHeight)
```

我们可以在我们创建的 `img` 元素上监听 `onload` 事件，以便在 canvas 上下文对象上实际绘制它：

```js
image.addEventListener('load', () => {
  ctx.drawImage(image, 0, 0, image.naturalWidth, image.naturalHeight)
})
```

现在，有了所有这些，我们就可以将内联 SVG 结果绘制到画布对象了。由于 canvas 的 `toBlob` 方法，我们可以生成一个可下载的文件。

## 使用 `toBlob` 创建可下载的 JPEG 或 PNG 图像

顾名思义，[`toBlob`](https://developer.mozilla.org/en-US/docs/Web/API/HTMLCanvasElement/toBlob) 方法将画布绘制的图像转换为 blob（[Blob](https://github.com/lio-zero/blog/blob/main/JavaScript/Blob%20%E5%AF%B9%E8%B1%A1.md) 是字符串的二进制表示），然后可以将其作为文件下载到您的计算机上。

首先，将回调函数作为 `toBlob` 参数，它接收 blob 本身，在回调中您可以继续对 blob 进行任何您想做的事情。

toBlob 还可以有 2 个附加参数：

- `type` — 它是一个 [MIME 类型](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Basics_of_HTTP/MIME_Types)，用于指定图片格式，默认格式为 `image/png`
- `quality` — 它需要一个介于 0（0% 质量）和 1（100% 质量）之间的数字，这在使用有损 MIME 类型（如 `image/jpeg`）时指定图片展示质量。

例如，这将创建一个质量为 90% 的 JPEG blob：

```js
canvas.toBlob(
  (blob) => {
    // do something
  },
  'image/jpeg',
  0.9
)
```

我们可以做的是使用 `URL.createObjectURL` 从内存中的 blob 创建一个 URL。

我们可以在页面上选择一个锚标签并将其 `href` 属性设置为 URL：

```js
canvas.toBlob(
  (blob) => {
    const anchor = document.getElementById('download-link')
    anchor.href = URL.createObjectURL(blob)
  },
  'image/jpeg',
  0.9
)
```

然后，用户只需单击锚点即可启动文件下载。

但是我们可以做得更好，让文件下载自动开始，所以我们将使用一个小技巧，我们以编程方式创建一个锚元素 `a`，并以编程方式单击它以自动触发下载：

```js
canvas.toBlob(
  (blob) => {
    const anchor = document.createElement('a')
    anchor.download = 'my-file-name.jpg'
    anchor.href = URL.createObjectURL(blob)

    anchor.click()

    // 将其从内存中删除并保存在内存中
    URL.revokeObjectURL(anchor.href)
  },
  'image/jpeg',
  0.9
)
```

您还会注意到，使用我们自动下载的图像，我们可以从内存释放/清理生成的 blob 到 URL 的映射，因为 blob 现在已作为文件下载到电脑上，因此我们完成了它。

一旦我们想要保存的内容被绘制到画布上下文中，`toBlob` 方法可以很容易地允许以最适合您的应用需求的任何格式和质量保存为文件。

[查看效果](https://code.juejin.cn/pen/7137572095852036110)
