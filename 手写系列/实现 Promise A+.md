# 实现 Promise/A+

本文的目标是编写一个与 [`promise`](https://github.com/then/promise/blob/master/src/core.js) 类似的符合 Promise/A+ 的实现。

以下前半部分译自 [Implementing promises from scratch](https://www.mauriciopoppe.com/notes/computer-science/computation/promises/)，也是本文的重点。你可以查看原文，它还使用 TDD 方式，编写一些测试用例，帮助你理解。下半部分是 Promise 各个方法的实现。

## Promise 状态

Promise 是必须处于以下状态之一的对象/函数：`PENDING`、`FULFILLED` 和 `REJECTED`，最初 promise 处于 `PENDING` 状态。

Promise 可以从 `PENDING` 状态转换为带 `value` 值的 `FULFILLED` 状态或带 `reason` 的 `REJECTED` 状态。

为了进行状态转换，promise 构造函数接收到一个名为 `executor` 的函数，`executor` 会立即被调用，调用时使用两个函数 `fulfill` 和 `reject` 来执行状态转换：

- `fulfill(value)` — 通过 `value` 从 `PENDING` 到 `FULFILLED`，`value` 现在是 promise 的属性。
- `reject(reason)` — 通过 `reason` 从`PENDING` 到 `REJECTED`，`reason` 现在是 promise 的属性。

最初的实现很简单：

```js
// 可能的状态
const PENDING = 'PENDING'
const FULFILLED = 'FULFILLED'
const REJECTED = 'REJECTED'

class APromise {
  constructor(executor) {
    // 初始化状态
    this.state = PENDING
    // 成功的 value 或拒绝的 reason 在内部映射为 value，最初 promise 没有值

    // 调用立即执行程序
    doResolve(this, executor)
  }
}

// 带 value 的 fulfill
function fulfill(promise, value) {
  promise.state = FULFILLED
  promise.value = value
}

// 带 reason 的 reject
function reject(promise, reason) {
  promise.state = REJECTED
  promise.value = reason
}

// 创建作为 executor 参数的 fulfill/reject  函数
function doResolve(promise, executor) {
  function wrapFulfill(value) {
    fulfill(promise, value)
  }

  function wrapReject(reason) {
    reject(promise, reason)
  }

  executor(wrapFulfill, wrapReject)
}
```

## 观察状态变化

为了观察 promise 状态的变化（以及成功的值或拒绝的原因），我们使用 `then` 方法，该方法接收两个参数，一个 `onFulfilled` 函数和一个 `onRejected` 函数，调用这些函数的规则如下:

- 当 promise 处于 `FULFILLED` 状态时，`onFulfilled` 函数将被调用，并带有 promise 履行的 `value`，例如 `onFulfilled(value)`
- 当 promise 处于 `REJECTED` 状态时，`onRejected` 函数将被调用，并带有 promise 被拒绝的 `reason`，例如 `onRejected(reason)`

我们将这些函数称为 promise `handlers`。

让我们将 `then` 函数添加到类原型中，注意它会根据 Promise 的状态调用 `onFulfilled` 或 `onRejected` 函数。

```js
class APromise {
  // ...
  then(onFulfilled, onRejected) {
    handleResolved(this, onFulfilled, onRejected)
  }
  // ...
}

function handleResolved(promise, onFulfilled, onRejected) {
  const cb = promise.state === FULFILLED ? onFulfilled : onRejected
  cb(promise.value)
}
```

## 单向转换

一旦转换到其中一个 `FULFILLED` 或 `REJECTED` 状态，promise 不得转换到任何其他状态。

在我们当前的实现中，调用 `executor` 的函数应该确保只调用一次 `fulfill` 或 `reject`，后续调用应该被忽略

```js
function doResolve(promise, executor) {
  let called = false

  function wrapFulfill(value) {
    if (called) return
    called = true
    fulfill(promise, value)
  }

  function wrapReject(reason) {
    if (called) return
    called = true
    reject(promise, reason)
  }

  executor(wrapFulfill, wrapReject)
}
```

## 处理 executor 错误

如果执行 `executor` 失败，promise 应转换到 `REJECTED` 状态，并说明失败原因

```js
function doResolve(promise, executor) {
  // ...
  try {
    executor(wrapFulfill, wrapReject)
  } catch (err) {
    wrapReject(err)
  }
}
```

## 异步 executor

如果解析器的 `fulfill/reject` 是异步执行，则我们的 `.then` 方法将失败，因为它的处理程序将立即执行。

让我们向 Promise 添加一个队列，它的目的是存储一旦 Promise 状态从 `PENDING` 更改为其他状态时将调用的处理程序，同时我们的 `.then` 方法应该检查 Promise 状态，以决定是立即调用处理程序还是存储处理程序，让我们将此逻辑移动到新的辅助函数 `handle`。

```js
class APromise {
  constructor(executor) {
    this.state = PENDING
    // 存在 .then 处理程序队列
    this.queue = []
    doResolve(this, executor)
  }

  then(onFulfilled, onRejected) {
    handle(this, { onFulfilled, onRejected })
  }
}

// 检查 promise 的状态：
// - 如果 promise 为 PENDING，将其推入 queue 以供以后使用
// - 如果 promise 还不是 PENDING，则调用处理程序
function handle(promise, handler) {
  if (promise.state === PENDING) {
    // 如果为 PENDING，推入 queue
    promise.queue.push(handler)
  } else {
    // 立即执行
    handleResolved(promise, handler)
  }
}

function handleResolved(promise, handler) {
  const cb =
    promise.state === FULFILLED ? handler.onFulfilled : handler.onRejected
  cb(promise.value)
}
```

此外，应该更新 `fulfill` 和 `reject` 方法，以便在调用时调用 Promise 中存储的所有处理程序，这将在更新状态和值后调用的新函数 `finale` 中实现。

```js
function fulfill(promise, value)
  // ...
  finale(promise)
}

function reject(promise, reason) {
  // ...
  finale(promise)
}

// 调用 promise 中存储的所有处理程序
function finale(promise) {
  const length = promise.queue.length
  for (let i = 0; i < length; i += 1) {
    handle(promise, promise.queue[i])
  }
}
```

## 链式的 Promise

我们的 `.then` 方法应该返回一个新的 Promise。

实现也很简单，但是我们将看到新的 Promise 以不同于使用 `executor` 的方式转换到不同的状态，新的 Promise 使用处理程序进行转换，如下所示：

- 如果 `onFulfilled` 或 `onRejected` 函数被调用
  - 如果执行时没有错误，Promise 将转换为 `FULFILLED` 状态，返回值作为 `value`
  - 如果执行时出现错误，Promise 将转换到 `REJECTED` 状态，并将错误作为 `reason`

让我们做一个 `.then` 方法首先返回 Promise：

```js
class APromise {
  // ...
  then(onFulfilled, onRejected) {
    // 空的 executor
    const promise = new APromise(() => {})
    handle(this, { onFulfilled, onRejected })
    return promise
  }
}
```

对于实现，我们首先必须将新的 Promise 也存储在处理程序队列中，这样，如果观察到的 Promise 被解析，那么队列中的元素就知道需要解析哪个 Promise。

```js
class APromise {
  // ...
  then(onFulfilled, onRejected) {
    const promise = new APromise(() => {})
    // 同时保存 promise
    handle(this, { promise, onFulfilled, onRejected })
    return promise
  }
}

function handleResolved(promise, handler) {
  const cb =
    promise.state === FULFILLED ? handler.onFulfilled : handler.onRejected
  // 根据规则执行处理程序和转换
  try {
    const value = cb(promise.value)
    fulfill(handler.promise, value)
  } catch (err) {
    reject(handler.promise, err)
  }
}
```

## 异步处理程序

接下来，让我们考虑处理程序返回 Promise 的情况，在这种情况下，作为处理程序一部分的 Promise（不是返回的 Promise）应该采用返回的 Promise 的状态 `value` 或 `reason`。

让我们设想以下场景：

```js
const executor = fulfill => setTimeout(fulfill, 0, 'p')
const p = new APromise(executor)

const qOnFulfilled = value =>
  new APromise(fulfill => fulfill(value + 'q'))
const q = p.then(qOnFulfilled)

const rOnFulfilled = value => (
  // 值应为 pq
)
const r = q.then(rOnFulfilled)
```

在我们当前的实现中，元组 `{ q，qOnFulfilled }` 存储在 `p` 的处理程序中，并且我们确信在将元组 `{ r, rOnFulfilled }` 存储在 `q` 之前调用了 `qOnFulfilled`，我们可以利用这一事实，并检测处理程序何时将 Promise 返回给返回的 Promise 中存储观察器，而不是在 `qOnFulfilled` 返回的 Promise 上存储 `{r，onFulfilled}`。

请注意，我们使用的是 `while`，因为嵌套的 Promise 本身可能有另一个 Promise 作为解析值。

```js
function handle(promise, handler) {
  // 接受最深处的 promise
  while (promise.value instanceof APromise) {
    promise = promise.value
  }

  // ...
}
```

## 其他情况

### 无效的处理程序

如果应该是函数的处理程序不是函数，我们的实现将失败

```js
const p = new APromise((fulfill) => fulfill('p'))
const qOnFulfilled = null
const q = p.then(qOnFulfilled)
```

在这种情况下，`q` 应该立即用 `p` 的值进行解析

```js
function handleResolved(promise, handler) {
  const cb =
    promise.state === FULFILLED ? handler.onFulfilled : handler.onRejected
  // 如果处理程序不是函数，则立即解析
  if (typeof cb !== 'function') {
    if (promise.state === FULFILLED) {
      fulfill(handler.promise, promise.value)
    } else {
      reject(handler.promise, promise.value)
    }
    return
  }
  // ...
}
```

### 在事件循环之后执行处理程序

处理程序是用一个新的堆栈调用的，此外，即使 `executor/handlers` 是同步的，也可以确保将来调用观察者，从而使 Promise 解决方案保持一致。

我们可以使用任何允许我们在事件循环之后调用函数的函数，这包括 `setTimeout`、`setImmediate` 和 `requestAnimationFrame`

```js
function handleResolved(promise, handler) {
  setImmediate(() => {
    // ...
  })
}
```

### 以已解决的 Promise 为理由拒绝

只有当 promise 不处于 `REJECTED` 状态时才采用嵌套 promise 的状态。

```js
function handle(promise, handler) {
  // 以返回的 promise 的状态为例
  while (promise.state !== REJECTED && promise.value instanceof APromise) {}
  // ...
}
```

### Promise 本身无法解决

在 `fulfill` 方法上，让我们检查履行值是否等于 Promise 本身，如果是这样，则抛出一个 `TypeError`：

```js
function fulfill(promise, value) {
  if (value === promise) {
    return reject(
      promise,
      new TypeError('A promise cannot be resolved with itself.')
    )
  }

  // ...
}
```

### Thenable

处理程序的返回值可能是一个 thenable，一个具有 `then` 属性的对象/函数，它是一个可访问的函数，`then` 函数就像一个 `executor`，它接收一个应用于转换 thenable 状态的 `fulfill` 和 `reject` 回调。

让我们修改 `fulfill` 方法并添加对 thenable 的检查，注意访问属性并不总是安全的操作（例如，属性可能使用 [`getter`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/get)），这就是为什么我们应该将它包装在 `try/catch` 中。

另外，thenable 的 `then` 应该被调用为 `this`

```js
function fulfill(promise, value) {
  if (value === promise) {
    return reject(
      promise,
      new TypeError('A promise cannot be resolved with itself.')
    )
  }

  if (value && (typeof value === 'object' || typeof value === 'function')) {
    let then
    try {
      then = value.then
    } catch (err) {
      return reject(promise, err)
    }

    // promise
    if (then === promise.then && promise instanceof APromise) {
      promise.state = FULFILLED
      promise.value = value
      return finale(promise)
    }

    // thenable
    if (typeof then === 'function') {
      return doResolve(promise, then.bind(value))
    }
  }

  // primitive
  promise.state = FULFILLED
  promise.value = value
  finale(promise)
}
```

## 实现 Promise.catch()

```js
class APromise {
  // ...
  catch(onRejected) {
    return this.then(null, onRejected)
  }
}
```

## 实现 Promise.finally()

> [`Promise.prototype.finally()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise/finally) 返回一个 `Promise`。在 promise 结束时，无论结果是 fulfilled 或者是 rejected，都会执行指定的回调函数。这为在 Promise 是否成功完成后都需要执行的代码提供了一种方式。

将返回 promise 的结果在 `then` 一次，无论返回的是 fulfilled 还是 rejected，都执行给定的回调函数。

```js
class APromise {
  finally(onFinally) {
    return this.then(
      /* onFulfilled */
      (res) => Promise.resolve(onFinally().call(this)).then(() => res),
      /* onRejected */
      (err) =>
        Promise.resolve(onFinally().call(this)).then(() => {
          throw err
        })
    )
  }
}
```

示例：

```js
new APromise((resolve, reject) => {
  resolve('ok')
})
  .then((res) => {
    console.log(res) // ok
  })
  .finally(() => {
    console.log('finally') // finally
  })
```

## 实现 Promise.resolve()

内部返回一个 promise，调用 `resolve` 方法，将 `value` 参数传入

```js
class APromise {
  // ...
  static resolve(value) {
    return new APromise((resolve) => resolve(value))
  }
}
```

## 实现 Promise.reject()

内部返回了一个 promise，调用 `reject` 方法，将 `reason` 参数传入

```js
class APromise {
  // ...
  static reject(reason) {
    return A((resolve, reject) => reject(reason))
  }
}
```

## 实现 Promise.race()

`Promise.race` 与 `Promise.all` 类似，但只等待第一个 settled 的 promise 并取得其 `result/error`，第一个 settled promise 之后，所有其他的 `result/error` 都会被忽略。

```js
class APromise {
  // ...
  static race(arr) {
    const _Promise = this
    if (!Array.isArray(arr)) {
      return _Promise.reject(new TypeError('race() only accepts an array'))
    }
    return new _Promise((resolve, reject) => {
      arr.forEach((p) => {
        _Promise.resolve(p).then(resolve, reject)
      })
    })
  }
}
```

示例：

```js
const sleep = (sm) =>
  new APromise((resolve) => setTimeout(() => resolve(sm), sm))
const err = (ms) => sleep(ms).then(() => APromise.reject(ms))

APromise.race([1, 2, 3]).then(console.log) // 1
APromise.race([sleep(300), sleep(100), sleep(200)]).then(console.log) // 100
APromise.race([sleep(3000), err(100), sleep(2000)]).catch(console.error) // Error: 100
APromise.race([err(50), err(60)]).catch(console.error) // 50
```

## 实现 Promise.all()

- 如果传入的可迭代对象内的 promise 全部成功，那么就返回 `resolve` 成功的数组
- 一旦有一个 promise 执行失败，`Promise.all` 直接返回错误的那个 `reject`。

```js
class APromise {
  // ...
  static all(promises) {
    let remaining = promises.length
    // 判断是否为空
    if (remaining === 0) return APromise.resolve([])

    return new APromise((resolve, reject) => {
      promises.reduce((acc, promise, i) => {
        APromise.resolve(promise).then(
          (res) => {
            acc[i] = res
            --remaining || resolve(acc)
          },
          (err) => {
            reject(err)
          }
        )
        return acc
      }, [])
    })
  }
}
```

测试：

```js
const sleep = (sm) =>
  new APromise((resolve) => setTimeout(() => resolve(sm), sm))
const err = (ms) => sleep(ms).then(() => APromise.reject(ms))

APromise.all([1, 2, 3]).then(console.log) // [1, 2, 3]
APromise.all([sleep(300), sleep(100), sleep(200)]).then(console.log)
APromise.all([sleep(3000), err(100), sleep(2000)]).catch(console.error) // 100
APromise.all([err(50), err(60)]).catch(console.error) // 50
```

## 实现 promise.any()

> [`Promise.any()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise/any) 接收一个 `Promise` 可迭代对象，只要其中的一个 `promise` 成功，就返回那个已经成功的 `promise`。如果可迭代对象中没有一个 `promise` 成功（即所有的 `promises` 都失败/拒绝），就返回一个失败的 `promise` 和 `AggregateError` 类型的实例。

`Promise.any` 的行为跟 `Promise.all` 刚好相反：

- 只要有一个成功，那么就立即 `resolve` 出去
- 如果全部失败，那么就将所有 `reject` 结果收集起来并返回 `AggregateError`

```js
class APromise {
  // ...
  static any(promises) {
    return A((resolve, reject) => {
      if (promises.length === 0)
        return reject(new AggregateError('All promises were rejected'))
      promises.reduce((acc, cur) => {
        Promise.resolve(cur).then(
          (data) => {
            resolve(data)
          },
          (err) => {
            acc.push(err)
            if (acc.length === promises.length)
              reject(new AggregateError('All promises were rejected'))
          }
        )
        return acc
      }, [])
    })
  }
}
```

示例：

```js
const sleep = (sm) =>
  new APromise((resolve) => setTimeout(() => resolve(sm), sm))
const err = (ms) => sleep(ms).then(() => APromise.reject(ms))

APromise.any([1, 2, 3]).then((o) => console.log(o)) // 1
APromise.any([sleep(3000), err(100), sleep(2000)]).then(console.info) // 2000
APromise.any([err(50), err(60)]).catch(console.log) // AggregateError
```

## 实现 Promise.allSettled()

> [`Promise.allSettled()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise/allSettled) 方法返回一个在所有给定的 promise 都已经 fulfilled 或 rejected 后的 promise，并带有一个对象数组，每个对象表示对应的 promise 结果。

```js
class APromise {
  // ...
  static allSettled(values) {
    let promises = [].slice.call(values)

    return new APromise((resolve, reject) => {
      let result = [],
        count = 0

      promises.forEach((promise) => {
        APromise.resolve(promise)
          .then((value) => {
            result.push({ status: 'fulfilled', value })
          })
          .catch((err) => {
            result.push({ status: 'rejected', value: err })
          })
          .finally(() => {
            if (++count === promises.length) {
              resolve(result)
            }
          })
      })
    })
  }
}
```

示例：

```js
const sleep = (sm) =>
  new APromise((resolve) => setTimeout(() => resolve(sm), sm))
const err = (ms) => sleep(ms).then(() => APromise.reject(ms))

APromise.allSettled([1, 2, 3]).then(console.log)
APromise.allSettled([sleep(300), sleep(100), sleep(200)]).then(console.log)
APromise.allSettled([sleep(3000), err(100), sleep(2000)]).catch(console.error)
APromise.allSettled([err(50), err(60)]).then(console.log)
```

## 更多资料

以上 Promise/A+ [完整示例](https://github.com/lio-zero/web-demos/blob/main/promise/es6-promise.js)。

- [JavaScript Promise 迷你书（中文版）](http://liubin.org/promises-book/)
- [Promises/A+](https://promisesaplus.com/)
- [Promises/A 与 Promises/A+ 的区别](https://promisesaplus.com/differences-from-promises-a)
- [Conformant Implementations](https://promisesaplus.com/implementations)
- [通过从头开始构建 Promise 来学习 JavaScript Promise](https://levelup.gitconnected.com/understand-javascript-promises-by-building-a-promise-from-scratch-84c0fd855720)
- [implementing](https://www.promisejs.org/implementing/)
- [Optimization killers](https://github.com/petkaantonov/bluebird/wiki/Optimization-killers)
- [Promise 实现原理](https://segmentfault.com/a/1190000013396601)
- [es6-promise](https://github.com/stefanpenner/es6-promise)
