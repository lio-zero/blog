# 什么是 CSRF 攻击？

> **[CSRF](https://zh.wikipedia.org/wiki/%E8%B7%A8%E7%AB%99%E8%AF%B7%E6%B1%82%E4%BC%AA%E9%80%A0)（Cross-site request forgery）：跨站请求伪造**。 是一种挟制用户在当前已登录的 Web 应用程序上执行非本意的操作的攻击方法。与 XXS 相比，XSS 利用的是用户对指定网站的信任，CSRF 利用的是网站对用户网页浏览器的信任。

## CSRF 的攻击原理

- 用户登录了受信任的网站，并在本地生成了 cookie。
- 在不退出登录的情况下，访问了危险网站。
- 危险网站会发出一个请求到受信任的网站，受信任的网站不知道请求是用户访问的还是危险网站访问的，由于浏览器会自动带上用户 cookie，所以受信任的网站会根据用户的权限处理危险网站所发出的请求，达到了模拟用户操作的目的。

## CSRF 的防范

- 对于 GET 的请求，我们不应对数据进行任何修改操作
- 请求时附带验证信息，比如**验证码**或者 `token`
- 对于 `cookie`，我们可以设置 `same-site`（`Lax` 或 `Strict`）来规定浏览器不能在跨域请求中携带 Cookie ，减少 CSRF 攻击
- 验证 HTTP `Referer` 字段，判断请求来源。`Referer` 指的是页面请求来源。意思是，**只接受本站的请求，服务器才做响应**；如果不是，就拦截。

## 更多资源

- [理解 CSRF(跨站请求伪造)](https://github.com/pillarjs/understanding-csrf/blob/master/README_zh.md)
- [Cross-Site Request Forgery Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.html)
- [Cookie 的 SameSite 属性](http://www.ruanyifeng.com/blog/2019/09/cookie-samesite.html)
- [前端安全系列之二：如何防止 CSRF 攻击？](https://juejin.cn/post/6844903689702866952)
- [安全问题：CSRF 和 XSS](https://github.com/poetries/FE-Interview-Questions/blob/master/%E5%AE%89%E5%85%A8%E9%97%AE%E9%A2%98%EF%BC%9ACSRF%E5%92%8CXSS.md)
- [Preventing CSRF and XSRF Attacks](https://blog.codinghorror.com/preventing-csrf-and-xsrf-attacks/)
