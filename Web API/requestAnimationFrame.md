# requestAnimationFrame

[`requestAnimationFrame()`](https://developer.mozilla.org/zh-CN/docs/Web/API/window/requestAnimationFrame) 是一个相对较新的浏览器 API。它提供了一种更可预测的方式来连接到浏览器渲染周期。

[`requestAnimationFrame` 浏览器支持情况](https://caniuse.com/requestAnimationFrame):

![`requestAnimationFrame` 浏览器支持情况](https://upload-images.jianshu.io/upload_images/18281896-b0b06e1d74ead659.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

它不是特定于动画的 API，但这是它使用最多的地方。

JavaScript 有一个[JavaScript 运行机制 — 事件循环（Event-Loop）](https://github.com/lio-zero/blog/blob/main/JavaScript/JavaScript%20%E8%BF%90%E8%A1%8C%E6%9C%BA%E5%88%B6%20%E2%80%94%20%E4%BA%8B%E4%BB%B6%E5%BE%AA%E7%8E%AF%EF%BC%88Event-Loop%EF%BC%89.md)。它不断运行以执行 JavaScript。

过去，动画是使用 `setTimeout()` 或 `setInterval()` 执行的。执行一点动画，然后调用 `setTimeout()` 在几毫秒后再次重复此代码：

```js
const performAnimation = () => {
  //...
  setTimeout(performAnimation, 1000 / 60)
}
setTimeout(performAnimation, 1000 / 60)
```

或者：

```js
const performAnimation = () => {
  //...
}
setInterval(performAnimation, 1000 / 60)
```

您可以通过获取超时或间隔引用并清除它来停止动画：

```js
let timer

const performAnimation = () => {
  // ...
  timer = setTimeout(performAnimation, 1000 / 60)
}

timer = setTimeout(performAnimation, 1000 / 60)

// ...

clearTimeout(timer)
```

`performAnimation()` 调用之间的 `1000 / 60` 间隔是由显示屏的刷新率决定，在大多数情况下，刷新率为 60 Hz（每秒重绘 60 次），因为如果显示屏由于其局限性而无法显示，那么执行重绘是没有用的。这将导致我们拥有大约 16.6ms 的时间来显示每一帧。

使用这种方法的问题在于，即使我们准确地指定了这个精度，浏览器可能正在忙着执行其他操作，而我们的 `setTimeout` 调用可能无法及时完成重绘，它将被延迟到下一个周期。

这很糟糕，因为我们丢失了一帧，而下一帧动画将执行 2 次，导致眼睛注意到笨拙的动画。

`requestAnimationFrame` 是执行动画的标准方式，它以一种不同寻常的方式工作，尽管代码看起来与 `setTimeout/setInterval` 代码非常相似：

```js
let request

const performAnimation = () => {
  request = requestAnimationFrame(performAnimation)
  // 使某物动画
}

requestAnimationFrame(performAnimation)

// ...

cancelAnimationFrame(request) // 停止动画
```

## 优化

因为 `requestAnimationFrame()` 的引入对 CPU 非常友好，如果当前窗口或选项卡不可见，则会导致动画停止。

在引入 `requestAnimationFrame()` 时，即使选项卡被隐藏，`setTimeout/setInterval` 也会运行，但现在，由于这种方法在节省电池方面被证明是成功的，浏览器也对这些事件实施了节流，允许每秒最多执行 1 次。

使用 `requestAnimationFrame` 浏览器可以进一步优化资源消耗，使动画更加流畅。

## 更多资料

- [JavaScript 中的 setTimeout 和 setInterval 方法](https://github.com/lio-zero/blog/blob/main/JavaScript/JavaScript%20%E4%B8%AD%E7%9A%84%20setTimeout%20%E5%92%8C%20setInterval%20%E6%96%B9%E6%B3%95.md)
- [Using requestAnimationFrame](https://css-tricks.com/using-requestanimationframe/)
- [Replacements for setInterval Using requestAnimationFrame](https://css-tricks.com/snippets/javascript/replacements-setinterval-using-requestanimationframe/)
- [TestUFO: Test your web browser for requestAnimationFrame() Timing Deviations](https://www.testufo.com/animation-time-graph)
