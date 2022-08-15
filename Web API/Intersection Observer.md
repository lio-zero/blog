# Intersection Observer

[`IntersectionObserver`](https://developer.mozilla.org/zh-CN/docs/Web/API/IntersectionObserver) 浏览器 API 将使我们能够轻松检测元素何时完全或部分出现在屏幕上。

当您需要禁用 UI 直到屏幕上显示某些内容时，Intersection Observer 非常方便。

例如，如果您使用过简书，您可能会发现当头部 `header` 滚动到一定区域时，会切换为另一个 `header`，以下是使用 Intersection Observer 实现的示例：

```js
const block = document.querySelector('.block')
const header = document.querySelector('.header')
const otherHeader = document.querySelector('.other-header')

const observer = new IntersectionObserver((entries) => {
  if (entries[0].intersectionRatio > 0) {
    header.classList.remove('switch-header')
    otherHeader.classList.remove('switch-other-header')
  } else {
    header.classList.add('switch-header')
    otherHeader.classList.add('switch-other-header')
  }
})

observer.observe(block)
```

[演示地址](https://codepen.io/lio-zero/pen/jOyQQrB)

`IntersectionObserver` 是浏览器原生提供的构造函数，接受两个参数：

- `callback` 是可见性变化时的回调函数
- `option` 是配置对象（可选）

构造函数的返回值是一个观察器实例。

```js
let io = new IntersectionObserver(callback, option)
```

实例的 `observe` 方法可以指定观察哪个 DOM 节点，需要观察多个节点，就要调用多次该方法：

```js
io.observe(el)
```

实例的 `unobserve` 方法可以停止观察：

```js
io.unobserve()
```

实例的 `disconnect` 方法可以关闭观察：

```js
io.disconnect()
```

以上是它实例的几个方法。

本来继续往下写的，但已经有很多关于它的教程了，这里推荐阮一峰老师的 [IntersectionObserver API 使用教程](http://www.ruanyifeng.com/blog/2016/11/intersectionobserver_api.html)。

以下是一些不错示例：

- [示例一](https://codepen.io/lio-zero/pen/ZEBJNQg)
- [示例二](https://codepen.io/michellebarker/full/xxwLpRG)
- [返回顶部](https://codepen.io/lio-zero/pen/JjEpajw)

还有很多示例，包括懒加载、无限滚动等，您可以进一步阅读下面的文章。

## 更多资料

关于 IntersectionObserver API 的文章很多，以下是一些不错的文章：

- [Intersection Observer comes to Firefox](https://hacks.mozilla.org/2017/08/intersection-observer-comes-to-firefox/)
- [Now You See Me: How To Defer, Lazy-Load And Act With IntersectionObserver](https://www.smashingmagazine.com/2018/01/deferring-lazy-loading-intersection-observer-api/)
- [Implementing Infinite Scroll And Image Lazy Loading In React](https://www.smashingmagazine.com/2020/03/infinite-scroll-lazy-image-loading-react/)
- [Using IntersectionObserver to Check if Page Scrolled Past Certain Point](https://css-tricks.com/using-intersectionobserver-to-check-if-page-scrolled-past-certain-point/)
- [Sticky Table of Contents with Scrolling Active States](https://css-tricks.com/sticky-table-of-contents-with-scrolling-active-states/)
- [How to Detect When a Sticky Element Gets Pinned](https://css-tricks.com/how-to-detect-when-a-sticky-element-gets-pinned/)
- [A Few Functional Uses for Intersection Observer to Know When an Element is in View](https://css-tricks.com/a-few-functional-uses-for-intersection-observer-to-know-when-an-element-is-in-view/)
- [Lazy Loading Images with Vue.js Directives and Intersection Observer](https://css-tricks.com/lazy-loading-images-with-vue-js-directives-and-intersection-observer/)
- [Styling Based on Scroll Position](https://css-tricks.com/styling-based-on-scroll-position/)
- [Parsing Markdown into an Automated Table of Contents](https://css-tricks.com/parsing-markdown-into-an-automated-table-of-contents/)
- [Lozad.js: Performant Lazy Loading of Images](https://css-tricks.com/lozad-js-performant-lazy-loading-images/)
- [Table of Contents with IntersectionObserver](https://css-tricks.com/table-of-contents-with-intersectionobserver/)
