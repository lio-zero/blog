# 开始使用 Canvas

`canvas` 是 HTML5 新增的标签，它就像一块幕布，可以用 JavaScript 在上面绘制各种图表、动画、游戏画面、数据可视化、图片编辑以及实时视频处理等方面。

```html
<canvas>您的浏览器不支持 Canvas</canvas>
```

`canvas` 默认是看不见的，为了帮助我们可视化 `canvas` 元素，我们为它添加一个边框：

```css
canvas {
  border: #333 10px solid;
}
```

效果如下：

![为 canvas 添加边框](https://upload-images.jianshu.io/upload_images/18281896-3dc8cffdcdbd92fa.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 调整我们的 `canvas` 元素

默认情况下，`canvas` 元素是宽 300 像素和高 150 像素，如果您需要更改 `canvas` 的大小，可以使用 `width` 和 `height` 属性：

```html
<canvas width="550" height="350"></canvas>
```

> 推荐：[style="width: ___" 与 width="___"](https://github.com/lio-zero/blog/blob/main/CSS/style%3D'width__'%20%E4%B8%8E%20width%3D'__'.md)

## 绘制线条

我们使用 [Canvas API](https://developer.mozilla.org/zh-CN/docs/Web/API/Canvas_API) 来画一条对角线。

首先，我们为 `canvas` 添加一个 `id`，方便我们使用 JS 去操作它：

```html
<canvas id="myCanvas" width="550" height="350"></canvas>
```

访问 `canvas` 元素：

```js
const canvas = document.querySelector('#myCanvas')
```

获取渲染上下文：

```js
const ctx = canvas.getContext('2d')
```

移动虚拟笔并指定起始坐标和结束坐标：

```js
ctx.beginPath()

ctx.moveTo(50, 50)
ctx.lineTo(450, 300)

// 绘制完后，关闭路径
ctx.closePath()
```

指定了绘制的线的厚度和颜色：

```js
ctx.lineWidth = 20
ctx.strokeStyle = 'plum'
```

最后，将线绘制在 `canvas` 上：

```js
ctx.stroke()
```

最终效果如下：

![使用 Canvas API 画一条直线](https://upload-images.jianshu.io/upload_images/18281896-e56812325cfd2c46.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
