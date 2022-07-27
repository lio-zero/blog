# 使用 JS 进行 API 身份验证

有一些接口不需要进行验证就可以使用，而有一些接口要求您在进行 API 调用之前验证你的身份。

## 如何验证 API

要对您进行身份验证，API 可能需要：

- 用户名和密码
- key 和 secret
- API key 或 OAuth token（令牌）

这些可以通过多种方式传递给 API。

## 作为查询字符串参数的凭据

一些 API 接受 API key 或其他凭据作为接口 URL 上的查询字符串参数。

例如：

```js
// 不要像这样在 JS 文件中存储凭据（这里只为了演示，开发时不会这么做）
let apiKey = '123456'

// 进行 API 调用
fetch(`https://example.com/v2/special?api-key=${apiKey}`)
  .then((response) => {
    if (response.ok) return response.json()
    throw response
  })
  .then((data) => render(data))
  .catch((error) => console.log(error))
```

再次提醒，您不应该像这样将 API 凭据存储在 JS 文件中。

## 带有基本身份验证的凭据

一些 API 使用简单的**用户名/密码**组合进行身份验证，使用一种称为**基本身份验证（basic auth）**的方法。

使用基本身份验证，可以在 `headers` 键上包含 `Authorization` 属性。对于它的值，可以使用以下 `Basic USERNAME:PASSWORD` 模式。

用户名和密码都需要进行 base64 编码，这可以通过 `window.btoa()` 方法实现。

```js
// 不要像这样在 JS 文件中存储凭据
let username = 'O.O'
let password = '123456'

// 身份验证（虚拟 API）
fetch('https://example.com/authenticate', {
  headers: {
    Authorization: `Basic ${btoa(username)}:${btoa(password)}`
  }
})
  .then((response) => {
    if (response.ok) return response.json()
    throw response
  })
  .then((data) => console.log(data))
  .catch((error) => console.warn(error))
```

使用基本身份验证，您通常会对特定的授权接口进行 API 调用。返回的 `data` 通常包括一个令牌，该令牌将在一段时间后失效，您将使用该令牌对其他接口进行任何调用。

同样，不应该在 JS 文件中包含用户名和密码。这些通常在某种类型的登录表单中被请求，然后传递给 API 而不被存储。

## 带有身份验证令牌的凭据

API 令牌被设计为短期凭证，您可以在通过其他方式（通常使用 key 和 secret 或用户名和密码）对自己进行身份验证后，使用它来验证 API 调用。

它可以持续几分钟、长达数月或者在某些情况下，无限期。

使用基于令牌的身份验证，您可以在 `headers` 键上再次包含 `Authorization` 属性。对于它的值，您可以使用 `Bearer TOKEN` 模式。它不需要进行编码。

```js
fetch('https://example.com/token', {
  headers: {
    Authorization: `Bearer ${token}`
  }
})
  .then((response) => {
    if (response.ok) return response.json()
    throw response
  })
  .then((data) => console.log(data))
  .catch((error) => console.warn(error))
```

关于具体的身份验证流程和它们的安全性防范，可以查看我写的另一篇文章。
