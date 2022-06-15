# Express 中的错误处理中间件

[Express 的错误处理中间件](http://expressjs.com/en/guide/error-handling.html)可帮助您处理错误，而无需重复同样的工作。假设您直接在 Express 路由处理程序中处理错误：

```js
app.put('/user/:id', async (req, res) => {
  let user
  try {
    user = await User.findOneAndUpdate({ _id: req.params.id }, req.body)
  } catch (err) {
    return res.status(err.status || 500).json({ message: err.message })
  }
  return res.json({ user })
})
```

上面的代码可以正常工作，但是，如果有数百个接口呢，那么错误处理逻辑将变得不可维护，因为它被重复了数百次。

## 定义错误处理中间件

Express 根据中间件函数采用的参数数量分为不同的类型。接受 4 个参数的中间件函数被定义为**错误处理中间件**，只有在发生错误时才会被调用。

```js
const app = require('express')()

app.get('*', function routeHandler() {
  // 此中间件抛出一个错误，Express 将直接转到下一个错误处理程序
  throw new Error('Oops!')
})

app.get('*', (req, res, next) => {
  // 此中间件不是错误处理程序（只有3个参数），Express 将跳过它，因为之前的中间件中存在错误
  console.log('这里不会打印')
})

// 您的函数必须接受 4 个参数，以便 Express 将其视为错误处理中间件。
app.use((err, req, res, next) => {
  res.status(500).json({ message: err.message })
})
```

Express 会自动为您处理同步错误，如上面的 `routeHandler()` 方法。但是 Express 不处理异步错误。如果出现异步错误，则需要调用 `next()`。

```js
const app = require('express')()

app.get('*', (req, res, next) => {
  // next() 方法告诉 Express 转到链中的下一个中间件。
  // Express 不处理异步错误，因此您需要通过调用 next() 来报告错误。
  setImmediate(() => {
    next(new Error('Oops'))
  })
})

app.use((err, req, res, next) => {
  res.status(500).json({
    message: err.message
  })
})
```

请记住，Express 中间件是按顺序执行的。您应该在所有其他中间件之后，最后定义错误处理程序。否则，您的错误处理程序将不会被调用：

## 与 `async/await` 一起使用

Express 无法捕获 `promise` 的异常，Express 在 ES6 之前编写，对于如何处理 `async/await` 它扔没有好的解决方案。

例如，下面的服务器永远不会成功发送 HTTP 响应，因为 Promise `reject` 永远不会得到处理：

```js
const app = require('express')()

app.get('*', (req, res, next) => {
  // 报告异步错误必须通过 next()
  return new Promise((resolve, reject) => {
    setImmediate(() => reject(new Error('woops')))
  }).catch(next)
})

app.use((error, req, res, next) => {
  console.log('will not print')
  res.json({ message: error.message })
})

app.listen(3000)
```

我们可以封装或者使用现有的库来进行捕获。

首先，我们先简单封装一个函数，将 `async/await` 与 Express 错误处理中间件联系起来。

> **注意**：异步函数会返回 Promise，因此您需要确保 `catch()` 所有错误并将其传递给 `next()`。

```js
function wrapAsync(fn) {
  return function (req, res, next) {
    fn(req, res, next).catch(next)
  }
}

app.get(
  '*',
  wrapAsync(async (req, res) => {
    await new Promise((resolve) => setTimeout(() => resolve(), 50))
    // Async error!
    throw new Error('woops')
  })
)
```

使用第三方库 [`express-async-errors`](https://www.npmjs.com/package/express-async-errors)，一个简单的 ES6 async/await 支持 hack：

```js
require('express-async-errors')
app.get('*', async (req, res, next) => {
  await new Promise((resolve) => setTimeout(() => resolve(), 50))
  throw new Error('woops')
})
```

## 最后

Express 错误处理中间件允许您以最大化关注点分离的方式处理错误。您不需要处理业务逻辑中的错误，如果使用 `async/await`，甚至不需要 `try/catch`。这些错误将出现在您的错误处理程序中，然后您的错误处理程序可以决定如何响应请求。确保在下一个 Express 应用程序中充分利用这一强大功能！
