# 实现 async/await

我们知道 `async/await` 是 `generator` 函数的语法糖，它的作用是用同步的方式，执行异步操作。

本文将看看如何实现一个 `async/await`。假定你已经熟悉 `async/await` 和 `generator` 函数。

首先，我们通过封装一个[高阶函数](https://github.com/lio-zero/blog/blob/master/JavaScript/JavaScript%20%E9%AB%98%E9%98%B6%E5%87%BD%E6%95%B0.md) `asyncToGenerator`，该函数接收一个 generator 函数作为参数 `generatorFn`，并经过一系列处理，最终返回一个具有与 `async` 相同功能的新函数。

我们知道，`async` 最终返回的是一个 Promise 对象，**那我们该如何返回一个 Promise 对象呢？**

```js
async function foo() {
  return 1
}

foo()
// Promise {<fulfilled>: 1}
```

熟悉 ES6 的你肯定立马想到答案，使用 Promise 构造函数包装一下：

```js
function asyncToGenerator(generatorFn) {
  return function () {
    return new Promise((resolve, reject) => {})
  }
}
```

在返回的函数中，调用了 generator 函数 `generatorFn`，并将返回值赋给变量 `gen`。然后返回一个 `Promise` 对象，并在其中定义了一个内部函数 `step`。

```js
function asyncToGenerator(generatorFn) {
  return function () {
    const gen = generatorFn.apply(this, arguments) // gen 有可能传参

    return new Promise((resolve, reject) => {
      function step(key, arg) {}
    })
  }
}
```

当调用 `step` 函数时，会执行 generator 函数的下一步。如果 generator 函数的执行已经完成，则会触发 `Promise` 对象的 `resolve` 方法，并将 generator 函数的返回值作为参数传入。如果 generator 函数的执行尚未完成，则会返回一个新的 `Promise` 对象，并继续调用 `step` 函数执行 generator 函数的下一步。

```js
function asyncToGenerator(generatorFn) {
  return function () {
    const gen = generatorFn.apply(this, arguments) // gen 有可能传参

    return new Promise((resolve, reject) => {
      function step(key, arg) {
        let ret

        // 可能存在返回 reject 状态的 Promise，使用 try...catch 捕获，并直接 reject
        try {
          ret = gen[key](arg)
        } catch (error) {
          return reject(error)
        }

        const { value, done } = ret

        // 如果 done 为 true，则表示 generator 函数的执行已经完成，返回 resolve(value)
        // value 是 generator 函数的返回值
        if (done) {
          return resolve(value)
        } else {
          // 如果 done 为 false，表示 generator 函数的执行尚未完成，
          // 返回一个新的 Promise 对象，并继续调用 step 函数执行 generator 函数的下一步。
          // 注意：value 有可能是成功或失败的，所以需要使用 Promise.resolve() 包装一下。
          return Promise.resolve(value).then(
            (val) => {
              step('next', val)
            },
            (err) => {
              step('throw', err)
            }
          )
        }
      }

      // 初次执行
      step('next')
    })
  }
}
```

这就是实现 `async/await` 的全部。

如果你使用 Babel 去转换一段 `async/await` 代码，得到的也是类似的实现。

在 generator 函数执行过程中，如果 generator 函数中有异步操作，可以返回一个 `Promise` 对象，然后在 generator 函数的下一步中使用 `yield` 关键字获取异步操作的结果。例如：

```js
function* generatorFn() {
  const data = yield Promise.resolve(123)
  return data
}
```

当调用返回的函数时，会执行 generator 函数的第一步，然后返回一个 `Promise` 对象。你可以通过调用 `then` 方法或使用 `await` 关键字来处理 generator 函数的返回值。

```js
asyncToGenerator(generatorFn)().then(console.log) // 123
await asyncToGenerator(generatorFn)()
```

需要注意，如果 generator 函数中的异步操作返回了一个失败的 `Promise` 对象，则会触发 `Promise` 对象的 `reject` 方法，并将错误信息传入。
