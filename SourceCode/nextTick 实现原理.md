# nextTick 实现原理

Vue [`nextTick`](https://github.com/lio-zero/blog/blob/main/Vue/Vue%20nextTick.md) 的实现基于 [JS 的运行机制](https://github.com/lio-zero/blog/blob/main/JavaScript/JavaScript%20%E8%BF%90%E8%A1%8C%E6%9C%BA%E5%88%B6%20%E2%80%94%20%E4%BA%8B%E4%BB%B6%E5%BE%AA%E7%8E%AF%EF%BC%88Event-Loop%EF%BC%89.md)，通过创建一个异步任务，`nextTick` 便可达到在同步任务后执行的目的。

## Vue 2 nextTick

Vue 2.6 nextTick 源码地址 [src/core/util/next-tick.js](https://github.com/vuejs/vue/blob/2.6/src/core/util/next-tick.js)。

源码做了两个操作：

- 环境判断
- nextTick 实现

### 环境判断/嗅探环境

这一步主要是判断使用哪个异步 API。依次判断：

- Promise 的 `then`
- `MutationObserver`
- `setImmediate`
- `setTimeout`

这里不贴代码了，太长。详细代码查看 [core/util/next-tick.js](https://github.com/vuejs/vue/blob/2.6/src/core/util/next-tick.js#L13-L85)。

按这个优先级顺序判断是因为 `microtask` 的优先级高，能确保队列中的微任务在一次事件循环前被执行完毕。vue 又考虑到了兼容性的问题，做了 `microtask` 向 `macrotask` 的降级方案。

> **Tips**：这个队列可能是 `microTask` 队列，也可能是 `macroTask` 队列。Promise 的 `then` 和 `MutationObserver` 属于微任务队列，而 `setImmediate` 和 `setTimeout` 属于宏任务队列。

依次检测它们是否存在，如果存在就使用它，以此来确定回调函数队列是以哪个 API 来异步执行。

### nextTick()

无论是 `Vue.nextTick()` 或 `this.$nextTick()` 都是调用 `nextTick()` 方法。

`nextTick` 的主要逻辑是，将传入的函数推入到一个全局的 `callbacks` 队列中，等待下一个任务队列时遍历 `callbacks`，执行相应的回调函数。

`nextTick` 的大致实现如下：

```js
let pending = false

// 存放需要异步调用的任务
const callbacks = []

function flushCallbacks() {
  pending = false
  // 循环执行队列
  const copies = callbacks.slice(0)
  // 清空队列
  callbacks.length = 0
  for (let i = 0; i < copies.length; i++) {
    copies[i]()
  }
}

function nextTick(cb) {
  // 这里减少了一些判断，详细内容请看源码
  callbacks.push(cb)

  // 根据 pending 来判断当前是否要执行 timerFunc
  if (!pending) {
    pending = true

    // 这里会调用第一步环境判断里的异步 API，一般都是 Promise，在源码中为 timerFunc
    // 所以我们使用 Promise 的 then 方法，在下一个微任务队列中把函数全部执行
    Promise.resolve().then(flushCallbacks)
  }
}
```

测试：

```js
nextTick(() => console.log(1))
nextTick(() => console.log(2))
nextTick(() => console.log(3))

console.log(4)

// 4 1 2 3
```

第一次调用 `nextTick` 时，`then` 已经运行，但 `flushCallbacks` 还没执行。

第二、三次会推入到 `callbacks` 队列中，接着执行同步函数，打印 4。

此时调用栈清空了，浏览器开始检查微任务队列，发现了 `flushCallbacks` 方法，执行它。

此时 `callbacks` 里的 3 个函数被依次执行。

从示例看来，其实理解了事件循环，也很好理解吧。

## Vue 3 nextTick

Vue 3 `nextTick` 核心逻辑不变，也是基于事件循环，只不过增加了几个专门维护队列的方法。

以下是 nextTick 的大致实现思路，其中简化了一些操作，核心实现不变。源码地址 👉 [`core/packages/runtime-core/src/scheduler.ts`](https://github.com/vuejs/core/blob/main/packages/runtime-core/src/scheduler.ts)。

首先，肯定是从 `nextTick` 函数入手：

```ts
const p = Promise.resolve()

// 利用 Promise 的 then 方法，在下一个微任务队列中把函数全部执行
function nextTick(fn) {
  return fn ? p.then(fn) : p
}
```

可以看出 `nextTick` 接受一个函数作为参数，同时会创建一个 Promise 微任务。当页面调用 `nextTick` 时，会把参数 `fn` 传递给 `.then`，在队列 `p` 的任务完成后，执行 `fn`。

Vue 3 中 `nextTick` 的队列由几个方法维护，基本执行顺序是：`queueJob` -> `queueFlush` -> `flushJobs` -> `nextTick` 参数的 `fn`。

## queueJob

`queueJob` 方法负责维护主任务队列，接受一个函数作为参数，为待入队任务，会将参数推入到 `queue` 队列中，推入前需要先进行唯一性的判断。

```js
const queue = []

function queueJob(job) {
  // 唯一性判断
  if (!queue.includes(job)) {
    queue.push(job) // 退出队列
    queueFlush() // 调用 queueFlush 创建微任务
  }
}
```

## queueFlush

`queueFlush` 方法负责尝试创建微任务，等待任务队列执行：

```js
// 是否正在等待执行
let isFlushPending = false

function queueFlush() {
  if (isFlushPending) return
  // 避免在事件循环周期内多次创建新的微任务
  isFlushPending = true
  nextTick(flushJobs)
}
```

这里需要注意，如果同时触发了两个组件更新的话，这里会触发两次 `nextTick`，也就是两次 `then`（微任务逻辑），但这是没有必要的，我们只需要触发一次即可处理完所有的 job 调用，所以需要判断一下，如果已经触发过 `nextTick` 了，那么后面就不需要再次触发一次 `nextTick` 逻辑了。

## flushJobs

`flushJobs` 方法负责执行队列任务，主要逻辑如下：

```js
function flushJobs() {
  isFlushPending = false // 是否正在等待执行
  let job
  // 循环执行队列，并清空
  while ((job = queue.shift())) {
    if (job) {
      job()
    }
  }
}
```

每次循环都从队列 `queue` 开头取出一个任务执行，直到 `queue` 为空。

还有 `flushPreFlushCbs` 方法负责前置任务队列和 `flushPostFlushCbs` 方法负责执行任务队列。
