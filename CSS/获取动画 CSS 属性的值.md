# 获取动画 CSS 属性的值

检查正在连续动画的值，我们可能第一时间想到的是 `setInterval`，但我们可以使用专门为动画而设计的 `requestAnimationFrame` 方法。

假设我们需要获取一个盒子 `box` 动画过后顶部的值，你使用如下操作：

```js
function updateValue() {
  const boxTopPosition = getComputedStyle(box).top
  console.log(imageTopPosition)

  requestAnimationFrame(updateValue)
}

requestAnimationFrame(updateValue)
```

**不使用 `Element.style.CSSPropertyName` 的原因**：它只获取由存在于每个元素上的 `style` 对象定义的值。换句话说，只有在 CSS 中明确定义或通过 JavaScript 设置的属性值才会出现。动画不属于这两类情况，所用使用它获取不到。

CSS `animation` 属性及其值仅在运行时在浏览器内部深处确定。涉及检查 `style` 对象的浅层方法将不起作用。

`requestAnimationFrame` 与浏览器的刷新率同步，所以每次帧更新都会调用 `updateValue` 方法。

> [查看示例](https://codepen.io/lio-zero/pen/qBoNdVG)
