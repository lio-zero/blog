# Node Koa 框架

[Koa](https://koajs.com/) 被定位为下一代 Web 框架，由 Express 原班人马打造。Koa 非常轻巧且模块化，可以在不使用回调的情况下编写代码。

Koa 应用程序是一系列中间件功能，可按顺序运行以处理传入的请求并提供响应。每个中间件功能都可以访问上下文对象，该上下文对象包装了原生 Node 请求和响应对象，并提供了改进的 API 来处理它们。

## 创建一个基于 Koa 的应用程序

安装 koa：

```bash
npm i koa
```

创建一个简单的服务器：

```js
const Koa = require('koa')
const app = new Koa()

app.use(async (ctx) => {
  ctx.body = 'Hello World'
})

app.listen(8080)
```

`app.listen` 方法指定监听端口，并启动当前应用。

`app.use` 用于注册中间件，将给定的中间件方法添加到当前的应用程序。其内部返回 `this`，因此可以链式调用。

```js
app.use(someMiddleware).use(someOtherMiddleware).listen(8080)
```

关于中间件的详细内容可查阅 [Middleware](https://github.com/koajs/koa/wiki#middleware)。

## Context 对象

Koa Content 将 node 的 `request` 和 `response` 对象封装在一个单独的对象里面，并且为编写 Web 应用程序和 API 提供了许多有用的方法。

```js
app.use(async (ctx) => {
  ctx // Context
  ctx.request // koa Request
  ctx.response // koa Response
})
```

许多 context 的访问器和方法为了便于访问和调用，简单的委托给他们的 `ctx.request` 和 `ctx.response` 所对应的等价方法，例如： `ctx.type` 和 `ctx.length` 委托给 `response` 对象，`ctx.path` 和 `ctx.method` 委托了 `request` 对象。

因此，您没有必要总是单独调用 `request` 或 `response` 对象来访问它们的属性。例如，如果您想访问请求的 `body`，只需直接执行即可：

```js
ctx.body // 它委托给 Koa 的 ctx.request
```

`context` 对象的全局属性和方法。

```js
ctx.req // Node 的 request 对象。
ctx.res // Node 的 response 对象。
ctx.request // Koa 的 Request 对象。
ctx.response // Koa 的 Response 对象。
ctx.app // 应用实例引用。
ctx.state // 用于在中间件传递信息。

ctx.throw() // 抛出包含 .status 属性的错误，默认为 500。该方法可以让 Koa 准确的响应处理状态。可以用 http-errors 来创建错误。
ctx.assert() // 抛出一个类似 .throw() 的错误。koa 使用 http-assert 来断言。它和 throw() 方法都直接绝定了 HTTP 响应的状态。
// 还有 ctx.cookies.get/set 下面也会讲到
```

Koa 不支持直接调用底层 `res` 进行响应处理。请避免使用以下 Node 属性：

```js
res.statusCode
res.writeHead()
res.write()
res.end()
```

上面属性都不能使用。

## Request 对象

Koa `Request` 对象是对 node 的 request 进一步抽象和封装，提供了日常 HTTP 服务器开发中一些有用的功能。如下都来自官网：

```js
ctx.header/ctx.headers 返回或设置一个对象，包含所有 HTTP 请求头信息
ctx.method  返回或设置 HTTP 请求的方法。
ctx.length 以数字形式返回 HTTP 请求的 Content-Length 属性，取不到值，则返回 undefined。

ctx.href 返回 HTTP 请求的完整路径，包括协议、端口和 url。
ctx.path 返回 HTTP 请求的路径，该属性可读写。
ctx.host 返回 HTTP 请求的主机（含端口号）。
ctx.hostname 返回 HTTP 的主机名（不含端口号）
ctx.origin 获取 URL 的来源，包括 protocol 和 host。
ctx.url 获取请求 URL。
ctx.originalUrl 获取请求原始 URL。
ctx.protocol 返回 HTTP 请求的协议，https 或者 http。
ctx.ip 返回发出 HTTP 请求的IP地址。

ctx.query 返回一个对象，包含了 HTTP 请求的查询字符串。如果没有查询字符串，则返回一个空对象。该属性可读写。
ctx.search 返回 HTTP 请求的查询字符串，含问号。该属性可读写。
ctx.querystring 返回 HTTP 请求的查询字符串，不含问号。该属性可读写。
ctx.fresh 返回一个布尔值，表示缓存是否代表了最新内容。通常与 If-None-Match、ETag、If-Modified-Since、Last-Modified 等缓存头，配合使用。
ctx.type 返回 HTTP 请求的 Content-Type 属性。
ctx.stale 返回 ctx.fresh 的相反值。
ctx.charset 返回 HTTP 请求的字符集。
ctx.socket 返回 HTTP 请求的 socket。
ctx.secure 返回一个布尔值，表示当前协议是否为 https。
ctx.subdomains 返回一个数组，表示HTTP请求的子域名。
ctx.is() 返回指定的类型字符串，表示 HTTP 请求的 Content-Type 属性是否为指定类型。
ctx.accepts() 检查 HTTP 请求的 Accept 属性是否可接受，如果可接受，则返回指定的媒体类型，否则返回 false。

ctx.acceptsEncodings() 该方法根据 HTTP 请求的 Accept-Encoding 字段，返回最佳匹配，如果没有合适的匹配，则返回 false。
ctx.acceptsCharsets() 该方法根据 HTTP 请求的 Accept-Charset 字段，返回最佳匹配，如果没有合适的匹配，则返回 false。
ctx.acceptsLanguages() 该方法根据 HTTP 请求的 Accept-Language 字段，返回最佳匹配，如果没有合适的匹配，则返回 false。
ctx.get() 返回 HTTP 请求指定的字段。
```

## Response 对象

Koa `Response` 对象是对 node 的 response 进一步抽象和封装，提供了日常 HTTP 服务器开发中一些有用的功能。如下都来自官网：

```js
ctx.body 返回或设置 HTTP 响应头信息。
ctx.status 返回或设置 HTTP 响应的状态码。默认情况下，该属性没有值。
ctx.message 返回或设置 HTTP 响应的状态信息。该属性与 ctx.response.message 是配对的。
ctx.length 返回或设置 HTTP 响应的 Content-Length 字段。该属性可读写，如果没有设置它的值，koa 会自动从 ctx.request.body 推断。
ctx.type 返回或设置 HTTP 响应的 Content-Type 字段，不包括 "charset" 参数的部分。
ctx.headerSent 该方法返回一个布尔值，检查是否 HTTP 回应已经发出。
ctx.redirect() 该方法执行 302 跳转到指定网址。
ctx.is() 该方法类似于 this.request.is()，用于检查 HTTP 响应的类型是否为支持的类型。
ctx.attachment() 该方法将 HTTP 响应的 Content-Disposition 字段，设为 "attachment"，提示浏览器下载指定文件。
ctx.set() 设置 HTTP 响应的指定字段。
ctx.get() 返回 HTTP 响应的指定字段。
ctx.append(field, value) 用值 value 附加额外的消息头 field。
ctx.remove() 移除 HTTP 响应的指定字段。
ctx.lastModified 该属性以 Date 对象的形式，返回 HTTP 响应的 Last-Modified 字段（如果该字段存在）。该属性可写。
ctx.etag   该属性设置 HTTP 响应的 ETag 字段。注意，不能用该属性读取 ETag 字段。
ctx.vary() 该方法将参数添加到 HTTP 响应的 Vary 字段。
```

## 中间件

> Koa 的中间件是一个级联式（Cascading）的结构，也就是说，属于是层层调用，第一个中间件调用第二个中间件，第二个调用第三个，以此类推。上游的中间件必须等到下游的中间件返回结果，才会继续执行，这点很像递归。

下面以 `'Hello World'` 的响应作为示例：

```js
const Koa = require('koa')
const app = new Koa()

// logger
app.use(async (ctx, next) => {
  await next()
  const rt = ctx.response.get('X-Response-Time')
  console.log(`${ctx.method} ${ctx.url} - ${rt}`)
})

// x-response-time
app.use(async (ctx, next) => {
  const start = Date.now()
  await next()
  const ms = Date.now() - start
  ctx.set('X-Response-Time', `${ms}ms`)
})

// response
app.use(async (ctx) => {
  ctx.body = 'Hello World'
})

app.listen(3000)
```

当请求开始时首先请求流通过 `logger` 和 `x-response-time` 中间件，然后继续移交控制给 `response` 中间件。

当其中一个中间件调用 `next()` 时，该函数暂停并将控制传递给定义的下一个中间件。当在下游没有更多的中间件执行后，堆栈将展开并且每个中间件恢复执行其上游行为。

也就是 `response` 执行完，没有 `next()` 了就往回走，接着来到 `x-response-time`，当它也执行完了，回到上游的 `logger` 执行完毕。

常用的一些中间件：

- [koa-router](https://github.com/ZijianHe/koa-router) 用于 koa 的路由器中间件。
- [koa-logger](https://github.com/koajs/logger) 开发风格的日志记录中间件。
- [koa-session](https://github.com/koajs/session) 用于 koa 的简单 session 中间件。
- [koa-jwt](https://github.com/koajs/jwt) 用于验证 JSON Web 令牌的 koa 中间件。
- [koa-bodyparser](https://github.com/koajs/bodyparser) 基于 [co-body](https://github.com/tj/co-body) koa body 解析器。支持 `json`、`form` 表单和 `text` 类型的 body。
- [koa-views](https://github.com/queckezz/koa-views) 用于 koa 的模板渲染中间件（hbs，swig，pug 等！ ✨）。
- [koa-ejs](https://github.com/koajs/ejs) koa 模板渲染中间件
- [koa-static](https://github.com/koajs/static) 静态文件服务器中间件。
- [koa-helmet](https://github.com/venables/koa-helmet) koa 安全标头，使您的应用更安全。
- [koa-compress](https://github.com/koajs/compress) 压缩 koa 中间件。
- [koa-convert](https://github.com/koajs/convert) 将基于 koa Generator 的中间件转换为基于 Promise 的中间件，老项目估计会用到。

用法都很简单，下面也会简单的列出几个示例。

## 错误处理

Koa 提供内置的错误处理机制，提供了 `ctx.throw()` 方法用来抛出错误：

```json
app.use(async (ctx) => {
  ctx.throw(500)
})
```

你也可以使用 `try...catch` 来自定义自己的错误处理机制

```js
app.use(async (ctx, next) => {
  try {
    await next()
  } catch (err) {
    ctx.status = err.statusCode || err.status || 500
    ctx.body = {
      msg: err.message
    }
  }
})
```

> **注意**：错误处理中间件必须放在所有其他中间件之前，这样你就可以抓取并记录你的应用可能面临的任何类型的错误，包括与其他中间件相关的错误。

如果有多个端点映射到的 JavaScript 文件，可以将该中间件集中在一个位置，并将其导出到所有端点。

对于未捕获错误，可以设置 `error` 事件监听函数。

```js
app.on('error', (err) => {
  console.error('server error', err)
})
```

`error` 事件监听函数还可以接受上下文对象，作为第二个参数。如果 `req` 或 `res` 期间出现错误，并且无法响应客户端，`context` 实例仍然被传递：

```js
app.on('error', (err, ctx) => {
  console.error('server error', err, ctx)
})
```

当发生错误并且仍然有可能响应客户端（即没有数据写入 `socket`）时，Koa 会以 500（内部服务器错误）进行适当响应，而不会导致进程停止，因此也就不需要 forever 这样的模块重启进程。

## 路由

可以通过 `this.path` 属性，判断用户请求的路径，从而起到路由作用。

```js
app.use(async (ctx) => {
  if (ctx.path === '/home') {
    ctx.body = 'Hello World'
  }
})
```

但在一般开发项目中，我们会使用到 `koa-router` 中间件。

```js
const Router = require('koa-router')
const router = new Router()

router.get('/', (ctx) => {
  ctx.body = 'Hello World!'
})

app.use(router.routes())
```

## 托管静态文件

安装：

```bash
npm i koa-static
```

使用：

```js
const serve = require('koa-static')

app.use(serve('.')) // 托管根目录下的所有文件、文件夹

app.use(serve(__dirname + '/public')) // 托管 public 目录下的所有文件
```

## 日志记录

安装：

```bash
npm i koa-static
```

使用：

```js
const Logger = require('koa-logger')

app.use(Logger())
```

![日志记录](https://upload-images.jianshu.io/upload_images/18281896-267c50325b1a3641.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

也可以自己自定义传输内容。自己看官网去！

## cookie

`cookie` 的读取和设置。

```js
ctx.cookies.get(name, [options])
ctx.cookies.set(name, value, [options])
```

`set` 和 `get` 都支持 `options` 配置参数。其中的 `signed` 参数，用于指定 cookie 是否加密。如果指定加密的话，必须用 `app.keys` 指定加密短语。

```js
app.keys = 'asdlfkhoiqwewqno'
ctx.cookies.set('userInfo', 1, { signed: true })
```

`signed` 如果为 `true`，则还会发送另一个后缀为 `.sig` 的同名 cookie，使用一个 27-byte url-safe base64 SHA1 值来表示针对第一个 [Keygrip](https://www.npmjs.com/package/keygrip) 键的 `cookie-name=cookie-value` 的哈希值。此签名密钥用于检测下次接收 cookie 时的篡改。

![cookies](https://upload-images.jianshu.io/upload_images/18281896-5b72306fbca9cc8e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

可配置参数如下：

> - `signed`：cookie 是否加密。
> - `expires`：cookie 何时过期
> - `path`：cookie 的路径，默认是 `/`。
> - `domain`：cookie 的域名。
> - `secure`：cookie 是否只有 https 请求下才发送。
> - `httpOnly`：是否只有服务器可以取到 cookie，默认为 `true`。

## session

```js
const koa = require('koa')
const app = koa()
const session = require('koa-session')

app.keys = ['some secret hurr'] // cookie 的签名

// 默认配置
app.use(session(app))

app.use(async (ctx) => {
  if (ctx.path === '/favicon.ico') return

  let n = ctx.session.views || 0
  ctx.session.views = ++n
  ctx.body = n + ' views'
})

app.listen(8080)
```

你也可以自定义配置：

```js
const CONFIG = {
  key: 'koa.sess', // (string) 默认的 cookie 秘钥
  // 如果 session cookie 被盗，此 cookie 将永远不会过期
  // "session" 将导致 cookie 在 session/浏览器关闭时过期
  maxAge: 86400000, // cookie 的最大过期时间，默认值为 1 天。
  autoCommit: true, // (boolean) 自动提交 headers (默认为 true)
  overwrite: true, // (boolean) 是否可以覆盖 (默认为 true)
  httpOnly: true, // (boolean) httpOnly 与否 (默认为 true)
  signed: true, // (boolean) 是否签名 (默认为 true)
  rolling: false, // (boolean) 每次请求强行设置 cookie。 (默认为 false)
  renew: false, // (boolean) 在 session 即将过期时续订 session，因此我们可以始终保持用户登录。(默认为 false)
  secure: true, // (boolean) 安全 cookie
  sameSite: null // (string) session cookie sameSite 选项 (默认为 null, 不设置)
}
```

详细请看 [koa-session](https://www.npmjs.com/package/koa-session)

## 跨域

安装：

```bash
npm i koa2-cors
```

使用：

```js
const route = require('koa2-cors')

app.use(cors())
```

## 更多资料

- [Migrating from Koa v1.x to v2.x](https://github.com/koajs/koa/blob/master/docs/migration.md)
- [Koa 框架教程](https://www.ruanyifeng.com/blog/2017/08/koa.html)
