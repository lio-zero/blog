# HTTP Cookie

通过使用 Cookie，我们可以在服务器和浏览器之间交换信息，以提供一种自定义用户会话的方式，并让服务器在请求之间识别用户。

HTTP 是无状态的，这意味着所有发送到服务器的请求源都是完全相同的，服务器无法确定请求是来自之前已经发送过请求的客户端，还是一个新的请求。

当 HTTP 请求发送时，浏览器会将 Cookie 随请求发送到服务器，然后从服务器返回，服务器可以编辑 Cookie 的内容。

**Cookie 本质上用于存储会话 id。**

过去，Cookie 用于存储各种类型的数据，因为没有其他选择。但现在有了 [Web Storage API](https://developer.mozilla.org/zh-CN/docs/Web/API/Web_Storage_API)（本地存储和会话存储）和 [IndexedDB](https://developer.mozilla.org/zh-CN/docs/Web/API/IndexedDB_API)，我们有了更好的选择。

尤其是因为 Cookie 所能保存的数据量非常有限，因为它会在每次 HTTP 请求到我们的服务器时来回发送，包括对图像或 CSS 或 JavaScript 文件等资源的请求。

Cookie 有着悠久的历史，它在 1994 年推出了第一个版本，随着时间的推移，它们在多个 RFC 修订版中被标准化。

> RFC（Request for Comments），是互联网工程任务组（IETF）定义标准的方式，IETF 是负责制定互联网标准的实体。

最新的 Cookie 规范在 2011 年的 [RFC 6265](https://tools.ietf.org/html/rfc6265) 中定义。

## Cookie 的限制

- Cookie 只能存储 4KB 的数据
- Cookie 是域私有的。网站只能读取其设置的 Cookie，不能读取其他域的 Cookie
- 每个域最多可以有 20 个 Cookie 限制（但具体数量取决于具体的浏览器实现）
- Cookie 的总数是有限的（但具体的数量取决于具体的浏览器实现）。如果超过这个数量，新的 Cookie 将替换旧的 Cookie。

可以在服务器端或客户端设置或读取 Cookie。

在客户端，Cookie 由文档对象公开为 `document.cookie`。

## 设置 Cookie

设置 Cookie 的最简单示例：

```js
document.cookie = 'name=O.O'
```

这将向现有 Cookie 添加一个新 Cookie（它不会覆盖现有的 Cookie）

Cookie 值应使用 `encodeURIComponent()` 进行 URL 编码，以确保其不包含任何在 Cookie 值中无效的空格、逗号或分号。

## 设置 Cookie 过期日期

如果您没有设置任何其他内容，Cookie 将在浏览器关闭时过期。为了防止出现这种情况，请添加一个到期日期，以 UTC 格式表示（例如 Mon, 12 Mar 2022 17:04:05 UTC）

```js
document.cookie = 'name=0.0; expires=Mon, 12 Mar 2022 17:04:05 UTC'
```

设置 24 小时后过期的 Cookie 的简单 JavaScript 片段：

```js
const date = new Date()

date.setHours(date.getHours() + 24)
document.cookie = 'name=O.O; expires=' + date.toUTCString()
```

或者，您可以使用 `max-age` 参数设置以秒数表示的过期时间：

```js
document.cookie = 'name=O.O; max-age=3600' // 60 分钟后过期
document.cookie = 'name=O.O; max-age=31536000' // 1年后过期
```

## 设置 Cookie Path

`path` 参数指定 Cookie 的文档位置，因此它被分配给特定路径，并且仅当路径与当前文档位置或父级匹配时才发送到服务器：

```js
document.cookie = 'name=O.O; path=/dashboard'
```

此 Cookie 发送到 `/dashboard`、`/dashboards/today` 和 `/dashboard/` 的其他子 URL 上发送，但不会发送到 `/posts` 上。

如果未设置路径，则默认为当前文档位置。这意味着要从内部页面应用全局 Cookie，需要指定 `path=/`。

## 设置 Cookie Domain

`domain` 参数可用于为 Cookie 指定子域。

```js
document.cookie = 'name=O.O; domain=mysite.com;'
```

如果未设置，它默认为 `host` 部分，即使使用子域（如果在 `subdomain.mydomain.com` 上，默认情况下它被设置为 `mydomain.com`）。域 Cookie 包含在子域中。

## Cookie 安全性

### Secure

添加 `Secure` 参数可以确保 Cookie 只能通过 HTTPS 安全传输，并且不会通过未加密的 HTTP 连接发送：

```js
document.cookie = 'name=O.O; Secure;'
```

请注意，这不会以任何方式使 Cookie 变得安全，始终避免向 Cookie 添加敏感信息

### HttpOnly

一个有用的参数是 `HttpOnly`，它使 Cookie 无法通过 `document.cookie` 访问，因此它们只能由服务器编辑：

```js
document.cookie = 'name=O.O; Secure; HttpOnly'
```

### SameSite

[`SameSite`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Set-Cookie/SameSite) 参数允许服务器要求 Cookie 不在跨站点请求上发送，而只在以 Cookie 域为源的资源上发送，这对降低 CSRF（跨站点请求伪造）攻击的风险有很大帮助。

```js
document.cookie = 'name=O.O; SameSite=None; Secure;'
```

它有三个值：

- `None` — 任何情况下都会向第三方网站请求发送 Cookie
- `Lax` — 只有导航到第三方网站的 GET 链接会发送 Cookie，跨域的图片、`iframe` 和 `form` 表单都不会发送 Cookie
- `Strict` — 任何情况下都不会向第三方网站请求发送 Cookie

目前，主流浏览器 `SameSite` 的默认值为 `Lax`，而在以前是 `None`，这将会预防大部分 CSRF 攻击，如果需要手动指定 `SameSite` 为 `None`，需要指定 Cookie `Secure` 属性，即在 HTTPS 下发送。

> 推荐：阮一峰老师的 [Cookie 的 SameSite 属性](http://www.ruanyifeng.com/blog/2019/09/cookie-samesite.html)或冴羽的[预测最近面试会考 Cookie 的 SameSite 属性](https://juejin.cn/post/6844904095711494151)。

## 更新 Cookie 值或参数

要更新 Cookie 的值，只需为 Cookie 名称分配一个新值：

```js
document.cookie = 'name=K.O'
```

与更新值类似，要更新过期日期，请使用新的 `expires` 或 `max age` 属性重新分配值：

```js
document.cookie = 'name=O.O; max-age=31536000' // 1 年后过期
```

只需记住，还要添加您最初添加的任何其他参数，如 `path` 或 `domain`。

## 删除 Cookie

要删除 Cookie，请取消设置其值并传递过去的日期：

```js
document.cookie = 'name=; expires=Thu, 01 Jan 1970 00:00:00 UTC;'
```

同样，还有您用来设置它的其他所有参数

## 访问 Cookie 值

要访问 Cookie，请使用 `document.cookie`：

```js
const cookies = document.cookie
```

这将返回一个字符串，其中包含为页面设置的所有 Cookie，以分号分隔：

```js
'name1=O.O; name2=K.O; name3=D.O'
```

## 检查 Cookie 是否存在

```js
// ES5
const hasName = document.cookie
  .split(';')
  .filter((item) => item.includes('name=')).length

if (hasName) {
  // name 存在
}
```

## 库

有许多不同的库将提供更友好的 API 来管理 Cookie。其中之一是 [js-cookie](https://github.com/js-cookie/js-cookie)，它最高支持 IE7，并且在 GitHub 上有很多 `star`（这总是很好）。

它的一些用法示例：

```js
Cookies.set('name', 'value')
Cookies.set('name', 'value', {
  expires: 7,
  path: '',
  domain: 'subdomain.site.com',
  secure: true
})

Cookies.get('name') // => 'value'
Cookies.remove('name')

// JSON
Cookies.set('name', { name: 'O.O' })
Cookies.getJSON('name') // => { name: 'O.O' }
```

**使用该 API 还是原生 Cookie API？**

这取决于您的选择，`js-cookie` 提供的 API 看起来更好，但它需要额外添加一定大小的包到项目中。

## 在服务器端使用 Cookie

用于构建 HTTP 服务器的每个环境都允许您与 cookie 进行交互，因为 Cookie 是现代 Web 的支柱，没有它们就无法构建很多内容。

这里有一个 Node.js 示例。

使用 Express.js 时，您可以使用 [`res.cookie`](http://expressjs.com/en/api.html#res.cookie) 创建 Cookie：

```js
res.cookie('name1', 'O.O', {
  domain: '.example.com',
  path: '/admin',
  secure: true
})

res.cookie('name2', 'K.O', {
  expires: new Date(Date.now() + 900000),
  httpOnly: true
})

res.cookie('name3', 'D.O', { maxAge: 900000, httpOnly: true })

// 负责序列化 JSON
res.cookie('name4', { items: [1, 2, 3] }, { maxAge: 900000 })
```

要解析 Cookie，一个好的选择是使用 [cookie-parser](https://github.com/expressjs/cookie-parser) 中间件。每个请求对象在 `req.cookie` 属性中都会有 Cookie 信息：

```js
req.cookies.name // O.O
req.cookies.age // 20
```

如果您使用 `signed: true` 创建 Cookie：

```js
res.cookie('name', 'O.O', { signed: true })
```

它将在 `req.signedCookies` 对象中可用。签名的 Cookie 受到保护，不会在客户端上进行修改。用于对 Cookie 值进行签名的签名确保您可以在服务器端知道客户端是否已对其进行了修改。

[session](https://github.com/expressjs/session) 和 [cookie-session](https://github.com/expressjs/cookie-session) 有两种不同的中间件选项用于构建基于 Cookie 的身份验证，使用哪种取决于您的需求。

## 使用浏览器开发工具检查 Cookie

DevTools 中的所有浏览器都提供了一个用于检查和编辑 Cookie 的界面。

Chrome：

![Chrome 开发工具 cookie](https://upload-images.jianshu.io/upload_images/18281896-e1f9f6c009ba1573.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

Firefox：

![Firefox 开发工具 cookie](https://upload-images.jianshu.io/upload_images/18281896-1640beb9d2c6cbe6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

另外，Google 浏览器的有一个 EditThisCookie 扩展用于管理 Cookie。您可以添加，删除，编辑，搜索，锁定和屏蔽 Cookie！

## Cookie 的替代品

有一种流行的技术，称为 JSON Web Tokens（JWT），它是一种基于 Token 的身份验证。

## 更多资料

- [Cookie，document.cookie](https://zh.javascript.info/cookie)
- [当浏览器全面禁用三方 Cookie](https://juejin.cn/post/6844904128557105166)
