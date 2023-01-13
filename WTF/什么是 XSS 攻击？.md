# 什么是 XSS 攻击？

> [XSS](https://zh.wikipedia.org/wiki/%E8%B7%A8%E7%B6%B2%E7%AB%99%E6%8C%87%E4%BB%A4%E7%A2%BC)（Cross Site Scripting）是跨站脚本攻击。它是一种网站应用程序的安全漏洞攻击，是代码注入的一种。它允许恶意用户将代码注入到网页上，其他用户在观看网页时就会受到影响。这类攻击通常包含了 HTML 以及用户端脚本语言。

## XSS 是前端问题还是后端问题？

两者都是。这是一个涉及前端和后端的网站架构问题。

## XSS 攻击原理

在 XSS 攻击中，恶意代码会被注入你的网站，然后被执行。例如：当注入具有渲染完整标签的属性的字符串时是可能的（如 `Element.innerHTML` 和 `Element.outerHTML`），而不仅仅是文本（如 `Node.textContent` 和 `Element.innerText`）。

不过，有一个内置的保护措施。

仅仅注入一个 `script` 标签不会让你受到攻击，因为你注入的 DOM 部分已经被解析并运行了。

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

进行编码以转义数据。这样做，浏览器将不会解析 JavaScript，因为例如 `<script>` 将被编码为 `&lt;script&gt;`。

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

escapeHTML('<script>alert("xss")</script>') // '&lt;script&gt;alert(&quot;xss&quot;)&lt;/script&gt;'
```

以上是一个简单的 HTML 转义函数示例，它对一些特殊字符转义。

它并不包含所有需要转义的内容，你可以使用 [DOMPurify](https://github.com/cure53/DOMPurify) 代替这段代码，它是一个专业的 HTML 转义库，支持转义所有可能的字符。

作为一般规则，应始终进行编码。

### 使用 XSS 库针对用户输入源过滤，设置标签白名单

你可以使用黑名单或白名单策略进行验证。不同之处在于：

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

在 HTTP 的响应头中设置 `set-cookie: httpOnly` 字段，让浏览器知道不能通过 `document.cookie` 的方式获取到 Cookie 内容。

例如，以下使用 Node.js Express 框架的 `express-session` 中间件来设置 `HttpOnly` 属性。

```js
const express = require('express')
const session = require('express-session')

const app = express()

app.use(
  session({
    name: 'session',
    secret: 'my secret',
    resave: false,
    saveUninitialized: true,
    cookie: {
      httpOnly: true,
      secure: true
    }
  })
)

app.listen(3000, () => {
  console.log('Server running on port 3000')
})
```

在这里，我们使用了 `session` 中间件来启用会话。我们通过在 Cookie 配置中添加 `httpOnly: true` 和 `secure: true` 来设置 `HttpOnly` 和 `secure` 属性。

需要注意的是，`secure` 属性表示只能在 HTTPS 上传输 Cookie, 你需要在你的网站上部署 HTTPS 才能使用它。

虽然避免了攻击者直接获取到 Cookie，但是攻击者仍然可以在页面内发起别的请求，直接篡改用户的信息。我们可以配合 token 或者验证码的形式来防止这种情况的发生。而这两种相比，验证码的安全性会更高一些。

> 推荐：[HTTP Cookie](https://github.com/lio-zero/blog/blob/main/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%BD%91%E7%BB%9C/HTTP%20Cookie.md)

## 设置 CSP 安全策略

> 内容安全策略（[CSP](https://developer.mozilla.org/en-US/docs/Web/HTTP/CSP)，Content Security Policy）用于检测和减轻用于 Web 站点的特定类型的攻击，例如 XSS 和数据注入等。

启用 CSP 的方式有 2 种。

- 添加 `meta` 标签 `http-equive`

```html
<meta
  http-equiv="Content-Security-Policy"
  content="default-src 'self'; img-src https://*; child-src 'none';"
/>
```

- 在 HTTP 响应头中设置 `Content-Security-Policy` 字段。

```http
Content-Security-Policy: default-src 'self'
```

这会告诉浏览器只允许加载来自当前网站的资源。

还可以设置更细粒度的策略，如指定允许脚本加载的域名，或者禁止使用 `eval` 等。

## 其他的攻击方式

除了本文所讲的 XSS 攻击外，还有：

- [CSRF 攻击](https://github.com/lio-zero/blog/blob/main/WTF/%E4%BB%80%E4%B9%88%E6%98%AF%20CSRF%20%E6%94%BB%E5%87%BB%EF%BC%9F.md) — 通过伪装来自可信来源的请求来实施恶意操作。
- SQL 注入攻击 — 通过插入恶意代码来破坏数据库。
- DDoS 攻击 — 通过利用大量请求来使网站瘫痪。
- 上传文件漏洞 — 通过上传恶意文件来获取服务器权限。
- DNS 查询攻击 — 通过欺骗 DNS 服务器来重定向流量到恶意网站。
- 会话劫持 — 通过盗用用户的会话标识来获取敏感信息。
- 钓鱼攻击 — 通过钓鱼链接和恶意网站来窃取用户数据。
- 命令注入攻击 — 通过插入恶意代码来执行系统命令。
- 文件包含漏洞 — 通过利用文件包含功能来访问系统文件。
- 密码破解攻击 — 通过暴力猜测和字典攻击来破解密码。

这些攻击方式都有不同的防御方法，需要综合考虑并采取适当的措施来防御。

## 总结

- 合适的 HTML 转义可以有效避免 XSS 漏洞，阻止浏览器将用户输入解析为实际 HTML，因此不会执行脚本。
- 对于不受信任的输入字段，限制其输入长度。
- 如果需要创建一个元素，请使用 `document.createTextNode()`。
- 如果需要向 HTML 标签添加属性，请使用 `setAttribute()` 方法。
- 如果需要向 URL 添加内容，请使用 `window.encodeURIComponent()` 方法。
- 如果需要向 HTML 元素添加内容，请使用 `textContent` 属性而不是 `innerHTML`，这样可以阻止浏览器通过 HTML 解析器运行字符串，而 HTML 解析器将在其中执行脚本。
- 针对用户输入源和不可信数据源必须进行过滤和校验。如 `<script>`、`<img>`、`<a>` 等标签要进行过滤。
- 在进行事件绑定时，应该避免使用一些可能被滥用的事件（如 `onLoad`），并且使用 `addEventListener()` 进行事件绑定会更安全，不要使用 HTML 元素的属性来进行事件绑定（如 `onclick`），因为这样可能会有潜在的安全隐患。最后，还应该确保事件处理程序是可信的。
- 避免拼接 HTML，采用比较成熟的渲染框架，如 Vue/React 等。
- 通过 CSP 安全策略和其他 HTTP 响应头的设置（如 `X-XSS-Protection` 和 `X-Content-Type-Options`）可以有效防止 XSS 攻击，但是需要注意它们可能会导致其他问题。
- 为 Cookie 设置 `httpOnly` 来禁止 JavaScript 读取某些敏感 Cookie，攻击者完成 XSS 注入后也无法窃取此 Cookie。
- 使用验证码可以防止脚本冒充用户提交危险操作，但是需要考虑可能的绕过方式，例如 OCR。
- 使用 XSS 攻击字符串和自动扫描工具寻找潜在的 XSS 漏洞，同时应该确保测试用例是最新的，并且覆盖了所有可能的输入类型和输出场景。
- 通过使用安全的 API 来防御 XSS 攻击，例如 [DOMPurify](https://github.com/cure53/DOMPurify)。

## 更多资料

- [前端安全系列（一）：如何防止 XSS 攻击？](https://tech.meituan.com/2018/09/27/fe-security.html)
- [web 安全之 XSS 攻击原理及防范](https://www.cnblogs.com/tugenhua0707/p/10909284.html)
- [Cross-site Scripting (XSS)](https://www.acunetix.com/websitesecurity/cross-site-scripting/)
- XSS 攻击脑图：[xss virus 1.0](https://raw.githubusercontent.com/phith0n/Mind-Map/master/xss%20virus%201.0.png)、[XSS 脑图](https://raw.githubusercontent.com/phith0n/Mind-Map/master/XSS%E8%84%91%E5%9B%BE.png) 和 [XSS2](https://raw.githubusercontent.com/phith0n/Mind-Map/master/XSS2.png)
- [5 ways to prevent code injection in JavaScript and Node.js](https://snyk.io/blog/5-ways-to-prevent-code-injection-in-javascript-and-node-js/)
- [XSS 前端防火墙 —— 内联事件拦截](http://fex.baidu.com/blog/2014/06/xss-frontend-firewall-1/)
- [XSS 前端防火墙 —— 可疑模块拦截](http://fex.baidu.com/blog/2014/06/xss-frontend-firewall-2/)
- [XSS 前端防火墙 —— 无懈可击的钩子](http://fex.baidu.com/blog/2014/06/xss-frontend-firewall-3/)
- [XSS 前端防火墙 —— 天衣无缝的防护](http://fex.baidu.com/blog/2014/06/xss-frontend-firewall-4/)
- [XSS 前端防火墙 —— 整装待发](http://fex.baidu.com/blog/2014/06/xss-frontend-firewall-5/)
