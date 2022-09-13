# co 源码解读

我相信您在很多文章中都有看到过说 `async` 是 Generator 的语法糖，**那是为什么呢？**

Generator 在运行过程中的每一步 `yield` 都需要通过 `gen.next` 来完成，而 `async/await` 简化了这种操作。

而今天要将的 co 库，它是一个基于 Generator 的流控制。更容易理解的说法是，它是 Generator 的一个自动执行器，通过递归自动处理过程中的每一步 `yield`，遇到错误就报错终止。

使用 Co、Promise 和 Generator，我们可以实现一个类似 `async/await` 行为的异步函数。

当然，co 库我们也可以自己实现，感兴趣的话可以看看我之前写的一篇[实现 async/await](https://github.com/lio-zero/blog/blob/main/%E6%89%8B%E5%86%99%E7%B3%BB%E5%88%97/%E5%AE%9E%E7%8E%B0%20async%E3%80%81await.md)。

co 源码地址 👉[co/index.js](https://github.com/tj/co/blob/master/index.js#L43-L106)，它的代码量很少，且大部分代码都是一些类型判断和数据转换的过程，这里我们截取其中的核心 `co` 函数，看看它是如何实现的。

```js
function co(gen) {
  var ctx = this // 保存当前环境上下文的 this
  var args = slice.call(arguments, 1) // 保存除了 gen 的所有参数

  return new Promise(function (resolve, reject) {
    // 如果 gen 传入的是一个函数，则调用 gen 函数并保存返回值
    if (typeof gen === 'function') gen = gen.apply(ctx, args)
    // 如果不存在返回值或者返回值的 next 不是一个函数，则直接返回 gen
    if (!gen || typeof gen.next !== 'function') return resolve(gen)
    // 先执行一下成功回调
    onFulfilled()

    // 成功的回调
    function onFulfilled(res) {
      var ret // 用来保存 gen.next 的返回值
      try {
        ret = gen.next(res)
      } catch (e) {
        return reject(e)
      }
      next(ret) // 把返回结果传入到 next 函数中
      return null
    }

    // 失败的回调
    function onRejected(err) {
      var ret
      try {
        ret = gen.throw(err)
      } catch (e) {
        return reject(e)
      }
      next(ret)
    }

    // 获取 generator 中的下一个值，并返回一个 promise
    function next(ret) {
      // 如果 done 为 true，则表示已经执行完了，直接 resolve(ret.value)
      if (ret.done) return resolve(ret.value)
      // 否则就把 ret.value 的返回值转换成 promise
      var value = toPromise.call(ctx, ret.value)
      // 转换成功后调用 then 方法继续执行 onFulfilled 和 onRejected 函数
      if (value && isPromise(value)) return value.then(onFulfilled, onRejected)
      // 如果返回值类型不对则直接报错
      return onRejected(
        new TypeError(
          'You may only yield a function, promise, generator, array, or object, ' +
            'but the following object was passed: "' +
            String(ret.value) +
            '"'
        )
      )
    }
  })
}
```

它主要是通过在两个回调（`onFulfilled` 和 `onRejected`）函数中执行 `next` 函数。

在 `next` 函数中通过 `value.then` 的方式在次执行两个回调，形成一个闭环。

一步步的自动执行所有的 `gen.next`，在 `ret` 的 `done` 为空的情况下跳出函数。
