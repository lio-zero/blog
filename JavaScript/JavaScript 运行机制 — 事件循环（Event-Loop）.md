# JavaScript 运行机制 — 事件循环（Event-Loop）

JavaScript 有一个基于[事件循环](https://developer.mozilla.org/en-US/docs/Web/JavaScript/EventLoop)的并发模型，事件循环负责执行代码、收集和处理事件以及执行队列中的子任务。

大家都知道 JS 是一门单线程的非阻塞的脚本语言。这也就意味着，代码在执行的任何时候只有一个主线程来处理所有的任务。所以弄懂事件循环机制对我们学习 JS 至关重要。

**JS 单线程基于事件循环**：分为异步任务和同步任务。同步任务执行完，在执行异步任务中的内容。

基本的执行步骤如下：

- 所有的同步任务都在主线程上执行，形成一个**执行栈（execution context stack）**。
- 主线程之外，还存在一个**任务队列（task queue）**。只要异步任务有了运行结果，就在**任务队列**中放置一个事件。
- 一旦**执行栈**中的所有同步任务执行完毕，系统会到**任务队列**读取并执行已经运行完且正在等待进入执行栈的任务。
- 上述过程会不断重复，也就是常说的 Event Loop（事件循环）。

其中，任务队列又分为**宏任务队列**和**微任务队列**。

- 当我们执行一个脚本时，它会自上而下运行
- 遇到同步代码按顺序执行，遇到宏任务放入宏任务队列，遇到微任务放入微任务队列
- 执行完当前宏任务，检查微任务队列，执行并清空微任务队列，如果在微任务的执行中又加入了新的微任务，也会在这一步一起执行
- 从任务队列取出下一个宏任务并执行

> **Tips**：其实完整的事件循环要复杂的多。另外，在每个微任务队列执行完之后，下一个宏任务开始之前，还存在一个渲染阶段。关于这些将不在本文进行介绍。您可以在最下面提供的文章找到答案。

最后，来看一下都有哪些宏任务和微任务队列：

- 宏任务（Macro Task）— 整体脚本代码、UI 渲染、I/O 操作、`postMessage`、`MessageChannel`、`requestAnimationFrame`、`setTimeout`、`setInterval`、`setImmediate`（Node.js 环境）
- 微任务（Micro Task）— Promise 的回调函数（`then()`、`catch()`、`finally()`）、`process.nextTick`（Node.js 环境）、`MutationObserver`

## 示例

下面是一段面试常考的题目：

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
  Promise.resolve().then(() => console.log(123213))
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

## 进一步阅读

- [HTML Living Standard — Event loops](https://html.spec.whatwg.org/multipage/webappapis.html#event-loops)
- [这一次，彻底弄懂 JavaScript 执行机制](https://juejin.cn/post/6844903512845860872)
- [事件循环：微任务和宏任务](https://zh.javascript.info/event-loop)
- [一次弄懂 Event Loop（彻底解决此类面试问题）](https://juejin.cn/post/6844903764202094606)
- [什么是 Event Loop？](http://www.ruanyifeng.com/blog/2013/10/event_loop.html)
- [JavaScript 运行机制详解：再谈 Event Loop](http://www.ruanyifeng.com/blog/2014/10/event-loop.html)
- [Nodejs Event Loop](https://stackoverflow.com/questions/10680601/nodejs-event-loop)
- [第 10 题：常见异步笔试题，请写出代码的运行结果](https://github.com/Advanced-Frontend/Daily-Interview-Question/issues/7)
- [JavaScript 的工作原理](https://blog.sessionstack.com/how-javascript-works-event-loop-and-the-rise-of-async-programming-5-ways-to-better-coding-with-2f077c4438b5)
- [Loupe](http://latentflip.com/loupe/?code=JC5vbignYnV0dG9uJywgJ2NsaWNrJywgZnVuY3Rpb24gb25DbGljaygpIHsKICAgIHNldFRpbWVvdXQoZnVuY3Rpb24gdGltZXIoKSB7CiAgICAgICAgY29uc29sZS5sb2coJ1lvdSBjbGlja2VkIHRoZSBidXR0b24hJyk7ICAgIAogICAgfSwgMjAwMCk7Cn0pOwoKY29uc29sZS5sb2coIkhpISIpOwoKc2V0VGltZW91dChmdW5jdGlvbiB0aW1lb3V0KCkgewogICAgY29uc29sZS5sb2coIkNsaWNrIHRoZSBidXR0b24hIik7Cn0sIDUwMDApOwoKY29uc29sZS5sb2coIldlbGNvbWUgdG8gbG91cGUuIik7!!!PGJ1dHRvbj5DbGljayBtZSE8L2J1dHRvbj4%3D) 一个事件循环可视化网站
- [深入解析你不知道的 EventLoop 和浏览器渲染、帧动画、空闲回调（动图演示）](https://juejin.cn/post/6844904165462769678)
- [Tasks, microtasks, queues and schedules](https://juejin.cn/post/6844903638238756878)
- [Understanding JavaScript Function Executions — Call Stack, Event Loop , Tasks & more](https://medium.com/@gaurav.pandvia/understanding-javascript-function-executions-tasks-event-loop-call-stack-more-part-1-5683dea1f5ec)
- [Event Loop and the Big Picture — NodeJS Event Loop Part 1](https://blog.insiderattack.net/event-loop-and-the-big-picture-nodejs-event-loop-part-1-1cb67a182810) — 作者写了一系列关于事件循环的文章，在文中有给出地址，这里不一一贴出来。
- [JS 原力覺醒 Day13 - Event Queue & Event Loop 、Event Table](https://ithelp.ithome.com.tw/articles/10221944)
