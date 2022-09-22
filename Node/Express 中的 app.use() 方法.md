# Express 中的 app.use() 方法

Express 应用程序有一个 [`use()` 方法](https://expressjs.com/en/api.html#app.use)。该方法向应用程序添加新的中间件。

例如，假设您想要打印 HTTP 方法（`get`、`post` 等）和每个请求的 URL。下面是如何添加一个新的中间件来打印每个请求的 HTTP 方法和 URL：

```js
const app = require('express')()

app.use((req, res, next) => {
  // 例如，对 /test 的 GET 请求将打印 GET /test
  console.log(`${req.method} ${req.url}`)
  next()
})

app.get('/test', (req, res, next) => {
  res.send('ok')
})

// 使用 Axios 测试上述应用程序
const server = await app.listen(5000)

const axios = require('axios')
// 打印 get /test
const res = await axios.get('http://localhost:5000/test')
```

## 中间件堆栈

在 Express 中，一切都是中间件。在内部，Express 应用程序有一个中间件堆栈，调用 `use()` 会在堆栈中添加一个新层。定义路由处理程序的函数，如 `get()` 和 `post()` 也会向堆栈添加层。**Express 按顺序执行中间件堆栈**，因此调用 `use()` 的顺序很重要。

例如，最常见的中间件功能之一是 [cors 中间件](https://www.npmjs.com/package/cors)，它将 CORS 标头附加到 Express HTTP 响应。在定义任何路由处理程序或任何其他发送 HTTP 响应的操作之前，请确保调用 `app.use(cors())`，否则将无法获得 CORS 标头！

```js
const app = require('express')()

// 此响应将没有 CORS 标头，因为顺序很重要。Express 将在此路由处理程序之后运行 CORS 中间件。
app.get('/nocors', (req, res) => {
  res.send('ok')
})

app.use(require('cors')())

// 此响应将具有 CORS 标头，因为此路由处理程序位于中间件列表中 CORS 中间件之后。
app.get('/cors', (req, res) => {
  res.send('ok')
})
```

另一个常见的中间件功能是 Express 的 [body-parser](http://expressjs.com/en/resources/middleware/body-parser.html)。该中间件负责解析请求体并设置 `req.body` 属性。确保在使用 `req.body` 之前调用 `app.use(express.json())`，否则它将是 `undefined` 的！

```js
const express = require('express')
const app = express()

// body 在 HTTP 响应中总是 undefined，因为 Express 将在这个路由处理程序之后运行 JSON body-parser。
app.post('/nobody', (req, res) => {
  res.json({ body: req.body })
})

app.use(express.json())

// body 将包含入站请求正文。
app.post('/body', (req, res) => {
  res.json({ body: req.body })
})
```

## `path` 参数

尽管 `use()` 方法通常仅使用 1 个参数调用，但您也可以向其传递一个 `path` 告诉 Express 在收到以给定 `path` 开头的 URL 请求时仅执行给定中间件。

```js
const app = require('express')()

app.use('/cors', require('cors')())

// 此响应没有 CORS 标头，因为路径 /nocors 不是以 /cors 开头的
app.get('/nocors', (req, res) => {
  res.send('ok')
})

// 此响应将具有 CORS 标头
app.get('/cors', (req, res) => {
  res.send('ok')
})

// 此响应也具有 CORS 标头，因为 /cors/test 以 /cors 开头
app.get('/cors/test', (req, res) => {
  res.send('ok')
})
```
