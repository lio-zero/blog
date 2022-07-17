# 将 canvas 打印到 Data URL

[Data URL](https://github.com/lio-zero/blog/blob/main/%E6%B5%8F%E8%A7%88%E5%99%A8/Data%20URL.md) 是浏览器提供的一项有用的功能。

通常，引用一张图片：

```html
<img src="image.png" />
```

它会在浏览器渲染到它时，发起请求获取到这张图片。

而 Data URL 将图像嵌入 HTML 本身，如下所示：

```js
<img src='data:image/png;base64,iVBORw0KGgoAA...' />
```

Canvas 对象的 `toDataURL()` 方法，将 `canvas` 生成图像的 Data URL：

```js
canvas.toDataURL()
```

我认为，当您创建服务器端映像并希望在服务器端生成的网页中提供它时，这尤其有用。

特别是，使用 [canvas](https://www.npmjs.com/package/canvas) 包，我们可以初始化一个画布：

```js
const { createCanvas, loadImage } = require('canvas')

const canvas = createCanvas(200, 200)
const ctx = canvas.getContext('2d')
```

例如，使用 `ctx.fillText('Hello，Canvas！', 50, 100)` 绘制，然后调用 `canvas.toDataURL()` 生成图像的 Data URL，然后我们可以将其以字符串形式附加到 HTML 中，如下所示：

```js
const imageHTML = `<img src="${canvas.toDataURL()}" />`
```

当然，您可以在前端执行相同的操作，但现在 `canvas` 对象是对要绘制的 HTML `<canvas>` 元素的引用：

```js
const canvas = document.getElementById('canvas')
// ...
const imageHTML = `<img src="${canvas.toDataURL()}" />`
```
