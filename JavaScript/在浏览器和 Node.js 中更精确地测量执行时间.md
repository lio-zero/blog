# 在浏览器和 Node.js 中更精确地测量执行时间

测量应用程序中的某个片段需要多长时间是很重要的。

在前端中，我们有 [Navigation TimingAPI](https://developer.mozilla.org/en-US/docs/Web/API/Navigation_timing_API)、[Resource Timing API](https://developer.mozilla.org/en-US/docs/Web/API/Resource_Timing_API) 以及 [User Timing API](https://developer.mozilla.org/en-US/docs/Web/API/User_Timing_API) 收集精确的指标。

此外，我们最常用的应该是使用 `Date` 对象来评估某件事情需要多长时间。例如：

```js
const before = Date.now()
console.log(before) // 1505722233092

// some code here
// ...

console.log(`${(Date.now() - before) / 1000}s`) // 10.845s
```

我可以用 `Date.now` 来计算时间。它返回一个 UNIX 时间戳。

> **注意**：`Date.now()` 方法返回自 1970 年 1 月 1 日 UTC 00:00:00 以来经过的毫秒数。

`console.time` 和 `console.timeEnd` 也是一种很常用的简便方法。在你不需要高分辨率计时的情况下，该测量时间的方式也是一种不错的选择。

```js
console.time('test')

// some code here
// ...

console.timeEnd('test') // testTime: 48.5732421875 ms
```

但如果我们想更精确一点呢？

## 浏览器中更精确的时间戳

在浏览器中，我们可以使用 [`performance.now()`](https://developer.mozilla.org/en-US/docs/Web/API/Performance/now) 方法，其返回一个精确到毫秒的 [`DOMHighResTimeStamp`](https://developer.mozilla.org/en-US/docs/Web/API/DOMHighResTimeStamp)。

`performance.now` 也可用于 [Web Worker](https://github.com/lio-zero/blog/blob/main/Web%20API/Web%20Worker.md) 或 [Service Worker](https://github.com/lio-zero/blog/blob/main//Web%20API/Service%20Worker.md)。在 Window 上下文中，返回的值是自 `navigationStart` 以来经过的时间。

```js
const before = performance.now()

// some code here
// ...

console.log(performance.now() - before)
console.log(`这需要 ${end - start}ms 才能完成`)
```

既然，我们说到了浏览器，那我们也应该来说一说 Node.js，其有什么方法可以用来测量时间？

## Node.js 中更精确的时间戳

Node.js `process` 模块中有一个名为 `hrtime.bigint()` 的方法以毫微秒为单位返回当前高分辨率实时值作为 `bigint`。

与 `process.hrtime()` 方法不同，它不支持额外的时间参数，因为差可以通过两个 `bigint` 的减法直接计算。

> **注意**：以前是直接使用的 `hrtime`，详细描述可查看 [Node.js 文档](https://nodejs.org/api/process.html#process_process_hrtime_time)。

以下是文档给出的一个代码示例：

```js
const start = process.hrtime.bigint()

setTimeout(() => {
  const end = process.hrtime.bigint()

  console.log(`基准测试花费了 ${Number(end - start) / 1000000} 毫秒`)
}, 1000)
```

## 尽可能精确地测量

为了方便起见，我们可以使用一些库来更加精准的测量我们的程序花费的多少时间：

> [hrtime](https://www.npmjs.com/package/hirestime) 是通用时间度量 API（Node 和浏览器）的简便封装器。在 Node 上使用 `process.hrtime()`，浏览器中的 **performance API**，如果两者都不可用，则返回到 `Date`。

默认情况下，时间以毫秒为单位：

```js
import hirestime from 'hirestime'

// 时间测量的起始点
const getElapsed = hirestime()

setTimeout((_) => {
  // 返回经过的毫秒数
  console.log(getElapsed())
}, 1000)
```

[hirestime](https://www.npmjs.com/package/hirestime) 是对 `process.hrtime.bigint()` 进行包装，它可以指定输出单位：

```js
import hirestime from 'hirestime'

// 时间测量的起始点
const getElapsed = hirestime()

setTimeout((_) => {
  // 返回经过的秒数
  console.log(getElapsed.s())
  console.log(getElapsed.seconds())

  // 返回经过的毫秒数
  console.log(getElapsed.ms())
  console.log(getElapsed.milliseconds())

  // 返回经过的纳秒数
  console.log(getElapsed.ns())
  console.log(getElapsed.nanoseconds())
}, 1000)
```
