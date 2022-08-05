# 在 HTML canvas 绘制文本

绘制文本有两种方式：

- `fillText(text, x, y)`
- `strokeText(text, x, y)`

下面，我们来看看如何在 `canvas` 上绘制文本。

我们先创建一个 `canvas` 标签，并设置基本的宽高：

```html
<canvas width="200" height="400"></canvas>
```

首先，获取对画布的引用：

```js
const canvas = document.querySelector('canvas')
```

并从中创建一个上下文对象：

```js
const ctx = canvas.getContext('2d')
```

[`getContext()`](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLCanvasElement/getContext) 方法根据作为参数传递的类型返回画布上的绘图上下文。

有效值为：

- `2d` 二维渲染上下文
- `webgl` 使用 WebGL 版本 1
- `webgl2` 使用 WebGL 版本 2
- 与 [`ImageBitmap`](https://developer.mozilla.org/en-US/docs/Web/API/ImageBitmap) 一起使用的 `bitmaprenderer`

[点击此处查看详细内容](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLCanvasElement/getContext#%E5%8F%82%E6%95%B0)。

现在我们可以调用 `ctx` 对象的 `fillText()` 方法：

```js
ctx.fillText('Hello, Canvas!', 100, 100)
```

`fillText` 方法的后两个参数分别为 `x` 和 `y` 的坐标。

在调用 `fillText()` 之前，还可以传递其他属性来自定义外观，例如：

```js
ctx.font = 'bold 20pt Menlo'
ctx.fillStyle = '#ccc'
ctx.fillText('Hello, Canvas!', 100, 100)
```

- `font` 属性更改字体样式
- `fillStyle` 属性更改图形的填充颜色。默认值是 `#000`。

如果画布看起来太小，你可以设置它的 `width` 和 `height` 属性：

```js
ctx.width = 600
ctx.height = 600
```

另一种绘制文本的方式：

```js
ctx.strokeStyle = '#ccc'
ctx.strokeText('Hello, Canvas!', 100, 100)
```

`strokeStyle` 属性更改图形的描边颜色。默认值是 `#000`。

`fillStyle` 和 `strokeStyle` 接受任何有效的 CSS 颜色，包括字符串和 RGB 计算。

您还可以更改与文本（`* = default`）相关的其他属性：

- `textAlign: start*, end, left, right, center` — 文本的对齐方式
- `textBaseline: top, hanging, middle, alphabetic*, ideographic, bottom` — 文本基线
- `direction: ltr | rtl | inherit*` — 文本方向

> 您可以在[此处](https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D)找到有关使用 `2d` 选项返回的所有 API 。

[演示地址](https://code.juejin.cn/pen/7128056881381113892)
