# process.nextTick() 在 Node.js 中是如何工作的？

[process](https://nodejs.org/api/process.html) 对象是 Node.js 核心 API 提供的少数全局对象之一。它可以从任何地方访问，因此它的方法也可以访问。

其中 [`process.nextTick()`](https://nodejs.org/docs/latest/api/process.html#process_process_nexttick_callback_args) 方法用于将函数的执行推迟到下一个[事件循环迭代](https://www.jianshu.com/p/02322f407a7b)。

浏览器 JavaScript 为我们引入了 `setTimeout()` 等函数，以便在不久的将来延迟任务。该函数的作用是：获取一个回调函数和一个表示回调函数执行时间的数值，以毫秒为单位。

浏览器 JavaScript 为我们引入了 `setTimeout()` 等函数，以便在不久的将来延迟任务。该函数的作用是：获取一个回调函数和一个表示回调函数执行时间的数值，以毫秒为单位。

```js
setTimeout(callback, 0)
```

在 Node.js 中，事件循环的每次迭代都称为一个记号。为了计划在事件循环的下一次迭代中调用回调函数，我们使用 `process.nextTick()`。它只接受一个没有时间限制的回调，因为它将在事件循环的下一次迭代中执行。

```js
process.nextTick(callback)
```

`setTimeout()` 和 `process.nextTick()` 之间的区别在于 `process.nextTick()` 函数特定于 Node.js 事件循环。

`setTimeout()` 使用 JavaScript 运行时调度自己的事件队列。当使用 `process.nextTick()` 时，事件循环在单个迭代中处理事件队列中的事件后，与之关联的回调函数将立即运行。与 `setTimeout()` 相比，它更快，因为队列与 `setTimeout()` 或 JavaScript 运行时关联。

```js
function cb() {
  console.log('在下一次迭代中处理')
}
process.nextTick(cb)
console.log('在第一次迭代中处理')

// result
// 在第一次迭代中处理
// 在下一次迭代中处理
```

上面的代码片段是 `process.nextTick()` 工作原理的一个示例。运行后，您会发现，第二个 `console.log` 打印在与函数 `cb()` 关联的 `console.log` 之前。
