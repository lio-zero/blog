# 实现 async/await

我们知道 `async/await` 是 `generator` 函数的语法糖，它的作用是用同步的方式，执行异步操作。

本文将看看如何实现一个 `async/await`。假定您已经熟悉 `async/await` 和 `generator` 函数。

首先，我们通过封装一个[高阶函数](https://github.com/lio-zero/blog/blob/master/JavaScript/JavaScript%20%E9%AB%98%E9%98%B6%E5%87%BD%E6%95%B0.md) `asyncToGenerator`，它接收一个 `generatorFn` 参数，并经过一系列处理，最终返回一个具有与 `async` 相同功能的函数。

而我们知道，`async` 最终返回的是一个 Promise 对象，**那我们该如何返回一个 Promise 对象呢？**

```js
async function foo() {
  return 1
}

foo()
// Promise {<fulfilled>: 1}
```

这样答案就很明确，直接调用 `new Promise()` 包装一下：

```js
function asyncToGenerator(generatorFn) {
  return function () {
    return new Promise((resolve, reject) => {})
  }
}
```

因为外部是用 `.then` 或 `await` 的方式去使用这个函数的返回值的。所以，我们在内部定义一个 `step` 函数，它用来一步步调用的 `yield`，其传递两个参数：

- `key` 有 `next` 和 `throw` 两种取值，分别对应了 `ret` 的 `next` 和 `throw` 方法
- `arg` 参数则是用来把 promise `resolve` 的值交给下一个 `yield`

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

        // 如果 done 为 true，则表示已经执行完了，直接 resolve(value)
        if (done) {
          return resolve(value)
        } else {
          // 如果 done 为 false，未执行完
          // 注意：value 有可能是成功或失败的
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
      step('next') // 初次执行
    })
  }
}
```

示例：

```js
const getData = (data) =>
  new Promise((resolve) => setTimeout(() => resolve(data), 1000))

function* gen() {
  const data = yield getData(1)
  console.log('data:', data)
  const data2 = yield getData(2)
  console.log('data2:', data2)
  return 'success'
}

const asyncFn = asyncToGenerator(gen)
asyncFn().then(console.log)

// 'data:' 1
// 'data2:' 2
```
