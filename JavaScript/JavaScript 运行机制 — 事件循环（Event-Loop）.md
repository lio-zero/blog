# JavaScript 运行机制 — 事件循环（Event-Loop）

JS 是一门单线程语言，最新的 HTML5 中新增了 **Web Worker**，但 JavaScript 是单线程这一核心仍未改变。

**JS 单线程基于事件循环**：分为异步和同步。同步执行完，在执行异步中的内容。

- 同步的进入主线程，异步的进入 Event Table（事件列表） 并注册函数。
- 当指定的事情完成时，Event Table 会将这个函数移入 Event Queue（事件队列）。
- 主线程内的任务执行完毕为空，会去 Event Queue 读取对应的函数，进入主线程执行。
- 上述过程会不断重复，也就是常说的 Event Loop（事件循环）。

## 更加精确的定义

任务队列又分为**宏任务队列**和**微任务队列**。

- 宏任务（MacroTask）：整体代码 Script、UI 渲染、I/O、`postMessage`、`MessageChannel`、`requestAnimationFrame`、`setTimeout`、`setInterval`、`setImmediate`（Node.js 环境）
- 微任务（MicroTask）：Promise 的回调函数（`then()`、`catch()`、`finally()`）、`process.nextTick`（Node.js 环境）、`MutaionObserver`

**事件循环**

1. 进入脚本执行宏任务，自上而下运行
2. 遇到同步代码按顺序执行，遇到宏任务放入宏任务队列，遇到微任务放入微任务队列
3. 执行完当前宏任务，执行微任务中执行完并正在等待执行的任务
4. 执行下一个宏任务，这样周而复始的执行顺序被称为事件循环

> **注意**：JS 的执行顺序和声明以及引用的顺序有关，先声明的顺序先执行，后声明的顺序后执行（如果有两个同名方法，后声明的会覆盖先声明的）

## 题目

```js
// 以下代码在 Node 环境运行：process.nextTick 由 Node 提供
console.log('1')
setTimeout(function () {
  console.log('2')
  process.nextTick(function () {
    console.log('3')
  })
  new Promise(function (resolve) {
    console.log('4')
    resolve()
  }).then(function () {
    console.log('5')
  })
})

process.nextTick(function () {
  console.log('6')
})

new Promise(function (resolve) {
  console.log('7')
  resolve()
}).then(function () {
  console.log('8')
})

setTimeout(function () {
  console.log('9')
  process.nextTick(function () {
    console.log('10')
  })
  new Promise(function (resolve) {
    console.log('11')
    resolve()
  }).then(function () {
    console.log('12')
  })
})

// 最终输出：1 7 6 8 2 4 3 5 9 11 10 12
```

> **注意**：Node 的 `process.nextTick` 有专门的 **nextTick 队列**运行，它总是在其他微任务之前运行。

## 更多资料

- [JavaScript 运行机制详解：再谈 Event Loop](http://www.ruanyifeng.com/blog/2014/10/event-loop.html)
- [什么是 Event Loop？](http://www.ruanyifeng.com/blog/2013/10/event_loop.html)
