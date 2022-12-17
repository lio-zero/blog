# JavaScript BOM

## 什么是 BOM？

**BOM**（Browser Object Model）即浏览器对象模型是一组对象，它们提供了与浏览器相关的功能，并且可以用来操作浏览器窗口以及与浏览器交互。

BOM 不是标准化的，最初是 Netscape 浏览器标准的一部分，它可以根据不同的浏览器进行更改。

BOM 与文档对象模型（DOM）是相互独立的，但是通常被用来一起操作网页。DOM 是一组对象，它们表示网页的结构和内容，而 BOM 则提供了对浏览器的控制。

例如，可以使用 BOM 的 `window` 对象来控制浏览器窗口的大小和位置，并使用 DOM 的 `document` 对象来操作网页中的元素。

## `window` 对象

所有浏览器都支持 [`window`](https://developer.mozilla.org/zh-CN/docs/Web/API/Window) 对象。它表示浏览器窗口，是 BOM 的根对象，代表整个浏览器环境。

所有 JavaScript 全局对象、函数以及 `var` 声明的变量（`let` 和 `const` 除外）均自动成为 `window` 对象的成员。另外，ES6 的 `class` 不会附加到 `window` 对象上。

> **注意**：在立即执行函数内，声明的函数和变量都不会附加到 `window` 对象上，但如果变量不声明，那么变量则会附加到 `window` 对象上。

使用 `window` 上的属性和方法时，可以不带 `window` 调用它们。比如 `window.location.href` 可以直接写成 `location.href`。

## 其他对象

除了上一节所说的 `window` 对象，还有：

- `location` 浏览器当前 URL 信息。
- `navigator` 表示浏览器的信息，如浏览器名称、版本号和用户代理字符串。
- `screen` 表示用户的屏幕信息，如分辨率和颜色深度。
- `history` 表示浏览器的历史记录，可以用来操作浏览器的前进、后退和加载历史记录中的网页。

详细可以阅读：

- [Location 对象](https://github.com/lio-zero/blog/blob/main/JavaScript/Location%20%E5%AF%B9%E8%B1%A1.md)
- [Navigator 对象](https://github.com/lio-zero/blog/blob/main/JavaScript/Navigator%20%E5%AF%B9%E8%B1%A1.md)
- [History API](https://github.com/lio-zero/blog/blob/main/Web%20API/History%20API.md)
- [Screen 对象](https://github.com/lio-zero/blog/blob/main/JavaScript/Screen%20%E5%AF%B9%E8%B1%A1.md)
