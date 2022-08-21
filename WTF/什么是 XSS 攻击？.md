# 什么是 XSS 攻击？

> [XSS](https://zh.wikipedia.org/wiki/%E8%B7%A8%E7%B6%B2%E7%AB%99%E6%8C%87%E4%BB%A4%E7%A2%BC)（Cross Site Scripting）是跨站脚本攻击。它是一种网站应用程序的安全漏洞攻击，是代码注入的一种。它允许恶意用户将代码注入到网页上，其他用户在观看网页时就会受到影响。这类攻击通常包含了 HTML 以及用户端脚本语言。

## XSS 是前端问题还是后端问题？

两者都是。这是一个涉及前端和后端的网站架构问题。

## XSS 攻击原理

在 XSS 攻击中，恶意代码会被注入您的网站，然后被执行。例如：当注入具有渲染完整标签的属性的字符串时是可能的（如 `Element.innerHTML` 和 `Element.outerHTML`），而不仅仅是文本（如 `Node.textContent` 和 `Element.innerText`）。

不过，有一个内置的保护措施。

仅仅注入一个 `script` 标签不会让你受到攻击，因为您注入的 DOM 部分已经被解析并运行了。

```js
// 这行不通
const div = document.querySelector('#app')
div.innerHTML = '<script>alert("XSS 攻击")</script>'
```

但是，稍后运行的 JavaScript 将继续运行（不是立刻执行的代码）。

在此示例中，我们尝试从无效来源加载图像。当它失败时，`onerror` 事件会运行一些恶意 JavaScript。

```js
// 这将运行
div.innerHTML = `<img src="/path/to" onerror="alert('XSS 攻击')">`
```

在这种情况下，它只是在提醒一条消息。但在真实的场景中，代码可能会从我们的网站上抓取敏感数据，并将其发送给第三方来源。

链接是另一个潜在的攻击媒介。如果 `href` 或 `src` 属性是从第三方数据中设置的，则有恶意的用户可以在 URL 前加上 `javascript:` 或 `data:text/html`，并在用户单击链接或元素加载时运行代码。

```js
div.innerHTML = `<a href="javascript:alert('另一个 XSS 攻击')">点我有惊喜</a>`
```

这些攻击方式不需要你做任何的登录认证，只是通过简单、合法的操作，向你的页面注入脚本。

## XSS 的攻击方式

- 反射型 XSS
- 存储型 XSS
- DOM 型 XSS

这里不过多介绍，关于 XSS 攻击方式可以查看最后附加的链接，我们重点来看看如何防范 XSS 攻击。

## XSS 防范措施

这里，我们介绍几种防范措施：

- 编码
- 验证
- CSP

### HTML 转义特殊字符

进行编码以转义数据。这样做，浏览器将不会解释 JavaScript，因为例如 `<script>` 将被编码为 `&lt;script&gt;`。

```js
const escapeHTML = function (handleString) {
  return handleString
    .replace(/&/g, '&amp;')
    .replace(/</g, '&lt;')
    .replace(/>/g, '&gt;')
    .replace(/ /g, '&nbsp;')
    .replace(/\'/g, '&#39;')
    .replace(/\"/g, '&quot;')
}

escapeHTML('<script>alert("xss")</script>')
```

作为一般规则，应始终进行编码。

### 使用 XSS 库针对用户输入源过滤，设置标签白名单

您可以使用黑名单或白名单策略进行验证。不同之处在于：

- 使用黑名单可以决定要禁止哪些标签
- 使用白名单可以决定允许哪些标签

白名单更安全，因为黑名单容易出错、复杂且不具有前瞻性。

这里，我们使用白名单指定的配置对不受信任的 HTML 进行清理（以防止 XSS），这里用到 [jsxss](http://jsxss.com/) 库：

```js
import xss from 'xss'
const html = xss('<script>alert("xss")</script>')

console.log(html)
```

### Cookie 设置 HttpOnly

在 HTTP 的响应头 `set-cookie` 时设置 `httpOnly`，让浏览器知道不能通过 `document.cookie` 的方式获取到 cookie 内容。

通过服务器设置：

```js
app.get('/', (req, res) => {
  if (req.cookies.isVisit) {
    console.log(req.cookies)
    res.send('欢迎再次光临')
  } else {
    res.cookie('isVisit', 1, { maxAge: 3600 * 1000, httpOnly: true })
    res.send('欢迎初次光临')
  }
})
```

虽然避免了攻击者直接获取到 Cookie，但是攻击者仍然可以在页面内发起别的请求，直接篡改用户的信息。我们可以配合 token 或者验证码的形式来防止这种情况的发生。而这两种相比，验证码的安全性会更搞些。

> 推荐：[HTTP Cookie](https://github.com/lio-zero/blog/blob/main/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%BD%91%E7%BB%9C/HTTP%20Cookie.md)

## 设置 CSP 安全策略

> 内容安全策略（[CSP](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/CSP)）用于检测和减轻用于 Web 站点的特定类型的攻击，例如 XSS 和数据注入等。

**启用 CSP 的方式有 2 种**：

- 添加 `meta` 标签 `http-equive`

```html
<meta
  http-equiv="Content-Security-Policy"
  content="default-src 'self'; img-src https://*; child-src 'none';"
/>
```

- 在服务器提供页面时，在响应头设置 `Content-Security-Policy`。

## 总结

- 合适的 HTML 转义可以有效避免 XSS 漏洞，阻止浏览器将用户输入解析为实际 HTML，因此不会执行脚本
- 对于不受信任的输入字段，限制其输入长度
- 如果需要创建一个元素，请使用 `document.createTextNode()`
- 如果需要向 HTML 属性添加内容，请使用 `setAttribute()` 元素的方法
- 如果需要向 URL 添加内容，请使用 `window.encodeURIComponent()` 方法
- 如果需要向 HTML 元素添加内容，请使用 `textContent` 属性而不是 `innerHTML`，可以阻止浏览器通过 HTML 解析器运行字符串，而 HTML 解析器将在其中执行脚本
- 针对用户输入源和不可信数据源必须进行过滤和校验。如 `<script>`、`<img>`、`<a>` 等标签要进行过滤
- 避免标签中使用 `onLoad` 等事件，在 JavaScript 中通过 `.addEventListener()` 事件绑定会更安全
- 避免拼接 HTML，采用比较成熟的渲染框架，如 Vue/React 等
- 通过 CSP 安全策略和其他 HTTP 响应头的设置等进一步确保安全性
- 为 cookie 设置 `httpOnly` 来禁止 JavaScript 读取某些敏感 Cookie，攻击者完成 XSS 注入后也无法窃取此 Cookie
- 使用验证码可以防止脚本冒充用户提交危险操作
- 使用 XSS 攻击字符串和自动扫描工具寻找潜在的 XSS 漏洞

## 更多资源

- [前端安全系列（一）：如何防止 XSS 攻击？](https://tech.meituan.com/2018/09/27/fe-security.html)
- [web 安全之 XSS 攻击原理及防范](https://www.cnblogs.com/tugenhua0707/p/10909284.html)
- [Cross-site Scripting (XSS)](https://www.acunetix.com/websitesecurity/cross-site-scripting/)
- XSS 攻击脑图：[xss virus 1.0](https://raw.githubusercontent.com/phith0n/Mind-Map/master/xss%20virus%201.0.png)、[XSS 脑图](https://raw.githubusercontent.com/phith0n/Mind-Map/master/XSS%E8%84%91%E5%9B%BE.png) 和 [XSS2](https://raw.githubusercontent.com/phith0n/Mind-Map/master/XSS2.png)
- [5 ways to prevent code injection in JavaScript and Node.js](https://snyk.io/blog/5-ways-to-prevent-code-injection-in-javascript-and-node-js/)
