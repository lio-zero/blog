# JavaScript BOM

## 什么是 BOM？

**BOM**（Browser Object Model）浏览器对象模型，允许 JavaScript 与浏览器 "对话"，是对浏览器本身进行操作的。

BOM 不是标准化的，最初是 Netscape 浏览器标准的一部分，它可以根据不同的浏览器进行更改。

## window 对象

所有浏览器都支持 [`window`](https://developer.mozilla.org/zh-CN/docs/Web/API/Window) 对象。它表示浏览器窗口。

所有 JavaScript 全局对象、函数以及 `var` 声明的变量（`let` 和 `const` 除外）均自动成为 `window` 对象的成员。另外，ES6 的 `class` 也不会附加到 `window` 对象上。

> **注意**：在立即执行函数内，声明的函数和变量都不会附加到 `window` 对象上，但如果变量不使用声明，那么变量则会附加到 `window` 对象上。

全局变量是 `window` 对象的属性。全局函数是 `window` 对象的方法。它提供与浏览器的交互 5 大属性，都是 `window` 对象的属性：

- `document` 文档对象
- `location` 浏览器当前 URL 信息
- `navigator` 浏览器本身信息
- `screen` 客户端屏幕信息
- `history` 浏览器访问历史信息

使用 `window` 上的属性和方法时，可以不带 `window` 调用它们。如：`window.document.body` 可以直接写出 `document.body`。

关于 `document` 和 `screen` 有空在细聊，关于其他的可以看：

- [Location 对象](https://github.com/lio-zero/blog/blob/main/JavaScript/Location%20%E5%AF%B9%E8%B1%A1.md)
- [Navigator 对象](https://github.com/lio-zero/blog/blob/main/JavaScript/Navigator%20%E5%AF%B9%E8%B1%A1.md)
- [History API](https://github.com/lio-zero/blog/blob/main/Web%20API/History%20API.md)
