# naturalWidth 与 width 的区别

`naturalWidth` 是元素的自然宽度，它永远不会改变。例如，即使使用 CSS 或 JavaScript 调整图像大小，100px 宽的图像的自然宽度始终为 100px。

而 `width` 是 `width` 属性的值。它可能会发生更改，并且可以通过 CSS 或 JavaScript 进行更新。

我们可以通过给定 `url` 加载的图像的获取自然宽度：

```js
const getNaturalWidth = (url) => {
  return new Promise((resolve, reject) => {
    const img = new Image()
    img.addEventListener('load', () => {
      resolve(img.naturalWidth)
    })
    img.addEventListener('error', () => reject())
    img.src = url
  })
}

getNaturalWidth('https://path/to/image.png').then((naturalWidth) => {
  // ...
})
```
