# 将文本写入 HTML canvas

一个 `canvas` 标签，并设置基本的宽高：

```html
<canvas width="200" height="400"></canvas>
```

首先，获取对画布的引用：

```js
const canvas = document.querySelector('canvas')
```

并从中创建一个上下文对象：

```js
const context = canvas.getContext('2d')
```

现在我们可以调用 `context` 对象的 `fillText()` 方法：

```js
context.fillText('Hello, Canvas!', 100, 100)
```

`fillText` 方法的后两个参数分别为 `x` 和 `y` 的坐标。

如果画布看起来太小，你可以设置它的 `width` 和 `height` 属性：

```js
canvas.width = 600
canvas.height = 600
```

在调用 `fillText()` 之前，还可以传递其他属性来自定义外观，例如：

```js
context.font = 'bold 70pt Menlo'
context.fillStyle = '#ccc'
context.fillText('hi!', 100, 100)
```
