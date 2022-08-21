# JSON Web Token

JSON Web Token（JWT）是用于为应用程序创建访问令牌（token）的标准。

它是这样工作的：

- 用户通常以用户名和密码的形式提供其凭据。
- 凭据将发送到服务器进行验证。
- 服务器生成一个令牌来验证用户身份，并将其发送给客户端。
- 令牌保存在浏览器中（通常保存在 cookie 或 `localStorage` 中），对于后续的每个请求，客户端都会将令牌发送回服务器，以便服务器知道请求来自特定的身份。

这种架构在现代 Web 应用程序中非常有效，在对用户进行身份验证后，我们对 REST 或 GraphQL API 执行 API 请求。

JWT 是经过加密签名的（但不是加密的，因此在 JWT 中存储用户数据时必须使用 HTTPS），因此我们可以保证在收到它时可以信任它，因为没有任何中间人可以在不使其无效的情况下拦截和修改它或它所持有的数据。

**JWT 的用途：主要用于 API 认证和服务器到服务器的授权。**

> 推荐：[使用 JS 进行 API 身份验证](https://github.com/lio-zero/blog/blob/main/JavaScript/%E4%BD%BF%E7%94%A8%20JS%20%E8%BF%9B%E8%A1%8C%20API%20%E8%BA%AB%E4%BB%BD%E9%AA%8C%E8%AF%81.md)。

## JWT 令牌是如何生成的？

JWT 由三部分组成：**Header（头部）、Payload（负载）、Signature（签名）**。

使用 Node.js，您可以使用以下代码生成令牌的第一部分：

```js
const header = { alg: 'HS256', typ: 'JWT' }
const encodedHeader = Buffer.from(JSON.stringify(header)).toString('base64')
```

我们将签名算法设置为 `HMAC SHA256`（JWT 支持多种算法），然后从这个 JSON 编码的对象创建一个缓冲区，并使用 base64 对其进行编码。

第一部分的结果是 `eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9`。

接下来，我们添加 payload，我们可以使用任何类型的数据对其进行自定义。有一些保留字段，包括 `iss` 和 `exp`，用于标识令牌的签发人和过期时间。

您可以使用对象将自己的数据添加到令牌中：

```js
const payload = { username: 'lio' }
```

我们将这个 JSON 编码的对象转换为 Buffer，并使用 base64 对结果进行编码，就像我们之前所做的那样：

```js
const encodedPayload = Buffer.from(JSON.stringify(payload)).toString('base64')
```

在这种情况下，第二部分结果为 `eyJ1c2VybmFtZSI6ImxpbyJ9`。

接下来，我们从 header 和 payload 内容中获得签名，这确保了即使我们的内容被拦截，也不会被更改，因为我们的签名将无效。为此，我们将使用 Node `crypto` 模块：

```js
const crypto = require('crypto')
const jwtSecret = 'secretKey'

const signature = crypto
  .createHmac('sha256', jwtSecret)
  .update(encodedHeader + '.' + encodedPayload)
  .digest('base64')
```

我们使用 `secretKey` 密钥，并创建加密签名的 base64 编码表示。

在我们的例子中，签名的值是：`vbaMqQKO16NNvbU1ttuwH1UmSKRlLMvyK/W8Xx1aV/s=`。

我们几乎完成了所有工作，我们只需要通过用点（`.`）分隔来连接 header、payload 和 signature 的 3 个部分：

```js
const jwt = `${encodedHeader}.${encodedPayload}.${signature}`
```

## API 认证

这可能是使用 JWT 的唯一合理方法。

一个常见的场景是：您注册了一个服务，并从服务仪表板下载了一个 JWT。从现在开始，您将使用它对服务器的所有请求进行身份验证。

另一个相反的用例是，当您管理 API 并且客户端连接到您时发送 JWT，并且您希望您的用户仅通过传递令牌来发送后续请求。

在这种情况下，客户端需要将令牌存储在某处，**但哪里是存储令牌的最佳选择呢？**

在这之前，我们需要先了解两种导致令牌被盗的常见攻击手段。

## 身份验证令牌漏洞

身份验证令牌可以提供比用户名和密码等永久凭据更高的安全性。但是如果令牌被盗，它可以用来访问另一个用户的帐户。

有两种类型的攻击可能导致身份验证令牌被盗：

- 在跨站脚本（XSS）攻击中，恶意 JavaScript 被注入到您的网站并运行。此代码可以从 `localStorage` 或 Cookie 中获取身份验证令牌，或将其发送给第三方。
- 在跨站请求伪造（CSRF）攻击中，HTTP 请求从第三方网站发送到您的网站（通常通过在恶意网站上提交隐藏表单）。如果用户已经登录到您的网站，他们的身份验证 Cookie 将自动发送，在他们不知情的情况下代表他们发起操作。

关于是否应该将身份验证令牌保存在 `localStorage` 或 Cookie 中以降低这些风险，存在很多争论。

> 推荐：[什么是 XSS 攻击？](https://github.com/lio-zero/blog/blob/main/WTF/%E4%BB%80%E4%B9%88%E6%98%AF%20XSS%20%E6%94%BB%E5%87%BB%EF%BC%9F.md)和[什么是 CSRF 攻击？](https://github.com/lio-zero/blog/blob/main/WTF/%E4%BB%80%E4%B9%88%E6%98%AF%20CSRF%20%E6%94%BB%E5%87%BB%EF%BC%9F.md)

## 您应该将身份验证令牌存储在哪里？

存储在 `localStorage` 中的令牌不会受到 CSRF 的攻击，因为 `localStorage` 不会随每个 HTTP 请求自动发送到服务器。但它容易受到 XSS 攻击，JavaScript 可以轻松访问它们。

```js
localStorage.setItem('token', 'abc123')
```

可以使用 `HttpOnly` 标志设置 Cookie。这会阻止 JavaScript 访问 Cookie，从而保护其免受 XSS 攻击。它仍然会随每个 HTTP 请求自动发送，因此仍然容易受到 CSRF 攻击。

许多文章建议，带有 `HttpOnly` 标志的 Cookie 是更安全的选项，因为虽然令牌可以使用，但它永远不会公开。

```js
document.cookie = 'token=abc123; secure; HttpOnly;'
```

然而，JavaScript 也不能用于设置带有 `HttpOnly` 标志的 Cookie。Cookie 只能由服务器设置。如果您的应用程序完全是客户端的，并且您使用 JavaScript 存储令牌，则不能使用此标志。在这种情况下，Cookie 与 `localStorage` 一样容易受到 XSS 攻击。

**那么，您应该使用哪一个？**

- 对于仅限客户端的应用程序，`localStorage` 实际上可能更安全，因为它不易受到 CSRF 攻击。
- 如果您可以使用服务器端代码获取和设置令牌，那么带有 `HttpOnly` 标志的 Cookie 可能是更好的选择。

> 推荐：[HTTP Cookie](https://github.com/lio-zero/blog/blob/main/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%BD%91%E7%BB%9C/HTTP%20Cookie.md)

## 选择最好的 JWT 库

根据您使用的语言和环境，您可以从多个库中进行选择。最受欢迎的是 [jwt.io](https://jwt.io/#libraries-io) 网站。

它提供了在线 JWT 调试器，并且提供了各种流行语言的相关库：

- [JST Debugger](https://jwt.io/#debugger)
- [JWT Libraries](https://jwt.io/#libraries-io)

## 不要将 JWT 作为会话令牌

您不应该将 JWT 用于会话。使用常规的服务器端会话机制，因为它效率更高，更不容易暴露数据。

## 什么是基于会话的身份验证，它有何不同？

在基于令牌的身份验证之前，保持用户登录的一种流行方式是基于会话的身份验证。

使用基于会话的身份验证时，仍会生成一个令牌来代表用户。服务器使用令牌设置一个 Cookie，该令牌会在后续每个请求中发送，就像基于令牌的身份验证一样。

但是，服务器不是将令牌和相关的用户信息存储在数据库中或使其自包含，而是在内存中维护一个包含所有用户详细信息的会话。

以下是一个简单示例：

```js
let sessions = {
  la9183651jbk: {
    username: 'abc@123.com',
    display: 'GangTe',
    permissions: ['admin', 'registration', 'homework']
  },
  ls8659183raw: {
    username: 'xyz@987.com',
    display: 'LinLee',
    permissions: ['registration', 'homework']
  }
}
```

这种方式有几个主要缺点。

对于具有许多用户的大型应用程序，在内存中保持许多会话处于活动状态会对服务器性能产生很大影响。将它们存储在数据库中，并仅在需要时访问它们，这样性能会更好。

如今，一个应用程序的各个部分分布在多个服务器上也很常见，有时使用第三方服务。由于会话存储在服务器内存中，因此基于会话的身份验证需要在该服务器上运行所有内容。

基于令牌的身份验证还提供了一种使用新令牌更新或刷新令牌的方法，以提高安全性。会话令牌通常在用户注销之前不会更改，这使得它们更不安全，更容易被欺骗。

基于会话的身份验证仍然存在，并且对于较小的应用程序来说仍然是一种完全有效的方法。

但是今天，基于令牌的身份验证是处理身份验证的标准方法，并以基于会话的身份验证无法满足的方式解决了现代 Web 应用的许多需求和挑战。

## 进一步阅读

- [JSON Web Token 入门教程](http://www.ruanyifeng.com/blog/2018/07/json_web_token-tutorial.html)
- [Introduction to JSON Web Tokens](https://jwt.io/introduction/)
- [Learn how to use JSON Web Tokens (JWT) for Authentication](https://github.com/dwyl/learn-json-web-tokens/blob/master/README.md)
- [Why JWTs Suck as Session Tokens](https://developer.okta.com/blog/2017/08/17/why-jwts-suck-as-session-tokens)
- [Stop using JWT for sessions](http://cryto.net/~joepie91/blog/2016/06/13/stop-using-jwt-for-sessions/)
- [Stop using JWT for sessions, part 2: Why your solution doesn’t work](http://cryto.net/~joepie91/blog/2016/06/19/stop-using-jwt-for-sessions-part-2-why-your-solution-doesnt-work/)
- [Branca](https://branca.io/) 一种安全令牌的数据格式，听说比 JWT 更安全，同类项目还有 [Paseto](https://paseto.io/)
- [JWT Common Attacks](https://blog.devgenius.io/jwt-common-attacks-b41de384113e)
