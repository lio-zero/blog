# process.nextTick() 在 Node.js 中是如何工作的？

当您试图理解 Node.js 事件循环时，其中一个重要部分是 [`process.nextTick()`](https://nodejs.org/docs/latest/api/process.html#process_process_nexttick_callback_args)。

> **Tips**：[process](https://nodejs.org/api/process.html) 对象是 Node.js 核心 API 提供的少数全局对象之一。它可以从任何地方访问，因此它的方法也可以访问。

在 Node.js 中，每次事件循环执行一次完整的过程，我们称之为 **tick**。

当我们将函数传递给 `process.nextTick()` 时，我们指示引擎在当前操作结束时，在下一个事件循环 **tick** 开始之前调用此函数：

```js
process.nextTick(() => {
  // do something ...
})
```

事件循环正忙于处理当前功能代码。

当此操作结束时，JS 引擎运行该操作期间传递给 `nextTick` 调用的所有函数。

我们可以通过这种方式告诉 JS 引擎异步处理函数（在当前函数之后），但要尽可能快，而不是让它排队。

这里我们讲一下，浏览器的 `setTimeout()` 函数，它用于在不久的将来执行延迟任务。

```js
setTimeout(() => {
  // do something ...
}, 0)
```

> 推荐：详细用法请看 [JavaScript 中的 setTimeout 和 setInterval 方法](https://github.com/lio-zero/blog/blob/main/JavaScript/JavaScript%20%E4%B8%AD%E7%9A%84%20setTimeout%20%E5%92%8C%20setInterval%20%E6%96%B9%E6%B3%95.md#javascript-%E4%B8%AD%E7%9A%84-settimeout-%E5%92%8C-setinterval-%E6%96%B9%E6%B3%95)。

先来看一个 🌰：

```js
process.nextTick(() => {
  console.log('nextTick')
})

console.log('log')

setTimeout(() => {
  console.log('setTimeout')
})

// log
// nextTick
// setTimeout
```

从上面的代码片段可以看出，`nextTick` 在 `setTimeout` 之前先被打印。

`setTimeout()` 和 `process.nextTick()` 之间的区别在于 `process.nextTick()` 函数特定于 Node.js 事件循环。

调用 `setTimeout()` 将在下一个 **tick** 结束时执行函数，这比使用 `nextTick()` 时要晚得多，后者将优先处理调用并在下一 **tick** 开始前执行。

所以，如果要确保在下一次事件循环迭代中代码已经执行，请使用 `nextTick()`。

## 更多资料

- [process.nextTick 与 setTimeout(fn, 0)](https://gist.github.com/mmalecki/1257394) — 一个基准测试
- [JavaScript 运行机制 — 事件循环（Event-Loop）](https://github.com/lio-zero/blog/blob/main/JavaScript/JavaScript%20%E8%BF%90%E8%A1%8C%E6%9C%BA%E5%88%B6%20%E2%80%94%20%E4%BA%8B%E4%BB%B6%E5%BE%AA%E7%8E%AF%EF%BC%88Event-Loop%EF%BC%89.md)
