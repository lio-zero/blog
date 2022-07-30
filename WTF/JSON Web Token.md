# JSON Web Token

JSON Web Token（JWT）是用于为应用程序创建访问令牌的标准。

它是这样工作的：

- 服务器生成一个令牌来验证用户身份，并将其发送给客户端。
- 对于后续的每个请求，客户端都会将令牌发送回服务器，以便服务器知道请求来自特定的身份。

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

我会将其存放在 HttpOnly cookie 中。其他方法都容易受到 XSS 攻击，因此应该避免使用。JavaScript 无法访问 HttpOnly cookie，它会在每次请求时自动发送到源服务器，因此它非常适合这个用例。

> 您也可以存储在 localStorage 或 sessionStorage 中，可以看看 [Token 一般存放在哪里？](https://juejin.cn/post/6922782392390746125)。

## 选择最好的 JWT 库

根据您使用的语言和环境，您可以从多个库中进行选择。最受欢迎的是 [jwt.io](https://jwt.io/#libraries-io) 网站。

它提供了在线 JWT 调试器，并且提供了各种流行语言的相关库：

- [JST Debugger](https://jwt.io/#debugger)
- [JWT Libraries](https://jwt.io/#libraries-io)

## 不要将 JWT 作为会话令牌

您不应该将 JWT 用于会话。使用常规的服务器端会话机制，因为它效率更高，更不容易暴露数据。

## 进一步阅读

- [JSON Web Token 入门教程](http://www.ruanyifeng.com/blog/2018/07/json_web_token-tutorial.html)
- [Why JWTs Suck as Session Tokens](https://developer.okta.com/blog/2017/08/17/why-jwts-suck-as-session-tokens)
- [Stop using JWT for sessions](http://cryto.net/~joepie91/blog/2016/06/13/stop-using-jwt-for-sessions/)
- [Stop using JWT for sessions, part 2: Why your solution doesn’t work](http://cryto.net/~joepie91/blog/2016/06/19/stop-using-jwt-for-sessions-part-2-why-your-solution-doesnt-work/)
- [Branca](https://branca.io/) 一种安全令牌的数据格式，听说比 JWT 更安全，同类项目还有 [Paseto](https://paseto.io/)
