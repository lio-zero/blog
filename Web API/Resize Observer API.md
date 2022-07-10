# Resize Observer API

[Resize Observer API](https://developer.mozilla.org/en-US/docs/Web/API/Resize_Observer_API) 提供了一种高性能的机制，通过该机制，代码可以监视元素的大小更改，并且每次大小更改时都会向观察者传递通知。

[Resize Observer API 支持情况](https://caniuse.com/?search=Resize%20Observer%20API)：

![Resize Observer API 支持情况](https://upload-images.jianshu.io/upload_images/18281896-a8f6e24e0ffffbf2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

响应迅速的 Web 应用程序会根据视口大小调整其内容。这通常是通过 CSS 和媒体查询来实现的。当 CSS 能力不够时，我们会使用 JavaScript。Javascript DOM 操作通过侦听 `window.resize` 事件与视口大小保持同步。

现代的 Web 应用程序通过一系列组件构成，这些组件也需要响应。以往的方法（CSS 媒体查询，JS `window.resize`，以及其他 Hack）无法跟踪组件的大小。

随着响应式 Web 应用的普及，对响应式组件的需求也会随之增长。ResizeObserver 的出现正是为组件提供响应大小变化的方式。

## 示例：调整图像大小

下面是一个使用 `ResizeObserver` 在调整图像大小的示例。

```js
const hasSupport = () => (window.ResizeObserver ? true : false)
const range = document.getElementById('range')
const img = document.getElementById('img')
const text = document.getElementById('resize-value')

function init() {
  if (hasSupport()) {
    let resizeObserver = new ResizeObserver(() => {})

    resizeObserver.observe(img)
  }

  return '不支持 Resize Observer API'
}

function resize(e) {
  const value = e.target.value

  img.style.width = `${value}%`
  text.textContent = `${value}%`
}

range.addEventListener('input', (e) => {
  resize(e)
})

init()
```

[示例地址](https://codepen.io/lio-zero/pen/poLvXde)

## 更多资料

- [resize-observer](https://github.com/WICG/resize-observer)
- [Resize Observer standard](https://drafts.csswg.org/resize-observer/)
