# CORS

在浏览器中运行的 JavaScript 应用程序通常只能访问为其提供服务的同一域（源）上的 HTTP 资源。

加载图片、脚本或样式总是有效的，但对另一台服务器的 XHR 和 Fetch 调用将失败，除非该服务器实现了允许该连接的方式。

这种方式被称为 CORS（Cross-Origin Resource Sharing），即**跨源资源共享**。

需要 CORS 的一件非常重要的事情是 ES 模块，如果你没有在服务器上设置允许服务第三方源的 CORS 策略，则请求将失败。

跨域资源在**访问不同的协议 + 域名（主域名、子域名）+ 端口**的情况下将失败。

这是为了你的安全，防止恶意用户利用 Web 平台。

但是，如果你同时控制服务器和客户端，那么你就有充分的理由允许它们相互通信。

## 如何控制？

CORS 通过在浏览器和服务器之间增加一些 HTTP headers 来实现跨域资源共享。具体来说，当一个资源从一个域（源）加载到另一个域（目标）时，浏览器会发送一个 `OPTIONS` 请求（称为**预检请求**）到目标域，用来确定是否允许跨域访问。服务器接收到 `OPTIONS` 请求后，可以返回一个 HTTP 响应头，告诉浏览器是否允许跨域访问。

具体来说，服务器会返回一个 `Access-Control-Allow-Origin` 响应头，用来指定允许跨域访问的源。如果这个响应头的值为 `*`，表示允许所有源进行跨域访问；如果这个响应头的值为具体的域名，则只有该域名所对应的源才可以继续进行跨域访问。

当然，CORS 还提供了一些额外的 HTTP 头，用来控制更细粒度的跨域访问。比如：

- `Access-Control-Allow-Methods` 响应头用来指定允许跨域访问的 HTTP 方法，比如 `GET`、`POST`、`PUT` 等。
- `Access-Control-Allow-Headers` 响应头用来指定允许跨域访问的 HTTP headers，比如 `Content-Type`、`X-Requested-With` 等。
- `Access-Control-Allow-Credentials` 响应头用来指定允许跨域访问的请求凭据，比如 Cookie、HTTP 认证参数等。
- etc

详细的 CORS 介绍请查阅阮一峰老师的[跨域资源共享 CORS 详解](https://www.ruanyifeng.com/blog/2016/04/cors.html)。

## 以 Express 为例

如果你使用 Node.js 和 Express 作为框架，可以使用 [CORS](https://github.com/expressjs/cors) 中间件，该中间件为我们处理了这一切。

查看 cors 源码 [lib/index.js](https://github.com/expressjs/cors/blob/master/lib/index.js)，其实现与上述所讲的一致，通过在 HTTP 响应中添加特定的响应头来解决跨域请求。

### 基本用法

以下是 Node.js Express 框架实现的简单服务器：

```js
const express = require('express')
const app = express()

app.get('/without-cors', (req, res, next) => {
  res.json({ msg: 'no CORS!' })
})

const server = app.listen(3000, () => {
  console.log('Listening on port %s', server.address().port)
})
```

如果 `/without-cors` 遇到来自不同来源的 `fetch` 请求，它将引发 CORS 问题。

![由于 CORS 政策，fetch 失败](https://upload-images.jianshu.io/upload_images/18281896-b5f5fb2ffbcb2362.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

如果要让请求正常运行，我们只需要导入 `cors` 并注册中间件：

```js
const express = require('express')
const cors = require('cors')
const app = express()

app.use(cors())

app.get('/with-cors', (req, res, next) => {
  res.json({ msg: 'xxx' })
})

// ...
```

当然，你也可以在单个路由中使用：

```js
app.get('/with-cors', cors(), (req, res, next) => {)
```

![请求成功](https://upload-images.jianshu.io/upload_images/18281896-19da6b82b17f8ece.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 仅允许特定来源

然而，这个例子有一个问题：**任何请求都将被服务器作为跨域接受**。

正如你在 **Network** 面板中看到的，传递的请求中有一个 `Access-Control-Allow-Origin: *`

![CORS 响应标头](https://upload-images.jianshu.io/upload_images/18281896-4ad5b5062f14f2d8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

你需要将服务器配置为只允许为一个源服务，并阻止所有其他源。

你可以这样做：

```js
const cors = require('cors')

// 注意，origin 的末尾不需要斜杆
const corsOptions = {
  origin: 'https://example.com'
}

app.get('/articles/:id', cors(corsOptions), (req, res, next) => {
  // ...
})
```

你还可以提供更多服务：

```js
const whitelist = ['http://example1.com', 'http://example2.com']

const corsOptions = {
  origin(origin, callback) {
    if (whitelist.indexOf(origin) !== -1) {
      callback(null, true)
    } else {
      callback(new Error('CORS 不允许'))
    }
  }
}
```

详细用法请[查阅文档](https://github.com/expressjs/cors#cors)。

## 预检

有一些请求是以简单的方式处理的。所有 `GET` 请求都属于这个组。

还有一些 `POST` 和 `HEAD` 请求也是如此。

如果 `POST` 满足使用 `Content-Type` 的要求，它们也将在这个组中：

- `application/x-www-form-urlencoded`
- `multipart/form-data`
- `text/plain`

所有其他请求都必须经过一个预批准阶段，即**预检（Preflight）**。浏览器这样做是为了确定它是否有权通过发出 `OPTIONS` 请求来执行某项操作。

预检请求包含一些服务器将用于检查权限的 headers：

```http
OPTIONS /resource/request
Access-Control-Request-Method: POST
Access-Control-Request-Headers: origin, x-requested-with, accept
Origin: https://example.com
...
```

服务器将响应如下内容：

```http
HTTP/1.1 200 OK
Access-Control-Allow-Origin: https://example.com
Access-Control-Allow-Methods: POST, GET, OPTIONS, DELETE
...
```

我们检查了 `POST`，但服务器告诉我们还可以为该特定资源发出其他 HTTP 请求类型。

按照上面的 Node.js Express 示例，服务器还必须处理 `OPTIONS` 请求：

```js
var express = require('express')
var cors = require('cors')
var app = express()

// 只允许在一个资源上使用 OPTIONS
app.options('/resource/request', cors())

// 在所有资源上允许 OPTIONS
app.options('*', cors())
```

## 最后

CORS 是一种安全机制，它用于在浏览器和服务器之间添加限制，以避免恶意的跨域请求。因此，在使用 cors 模块时，应该根据实际情况设置适当的 CORS 策略，以保证应用的安全性。

## 更多资料

[How to win at CORS](https://jakearchibald.com/2021/cors/)
