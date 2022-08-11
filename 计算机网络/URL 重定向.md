# URL 重定向

[URL 重定向](https://zh.wikipedia.org/wiki/%E7%B6%B2%E5%9F%9F%E5%90%8D%E7%A8%B1%E8%BD%89%E5%9D%80)（URL redirection）—— 一种将网站访问者从一个 URL 转移到另一个 URL 的 Web 服务器技术。当用户访问其浏览器中的某个 URL 时，服务器会发回一条消息，告诉浏览器改为访问其他 URL。第二个 URL 可以在同一个域上，也可以在不同的域上。

有两种常见的 URL 重定向类型。

- **301（Moved Permanently）** — 告诉浏览器（和搜索引擎）重定向是永久性的，并且他们将来应该使用更新的 URL。
- **302（Moved Temporarily）** — 表示重定向是临时的；浏览器和搜索引擎应继续请求原始 URL。

![URL 重定向](https://www.elated.com/wp-content/uploads/2016/10/url-redirection.png)

## 小技巧

如果有一个令人讨厌的 301 重定向无法清除，请打开开发工具勾选 **disable cache** 选项：

![disable cache](https://upload-images.jianshu.io/upload_images/18281896-507706cec008ae17.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 更多资料

- [永久性重定向（301）和临时性重定向（302）对 SEO 有什么影响](https://github.com/Advanced-Frontend/Daily-Interview-Question/issues/241)
- [HTTP 状态码 301 和 302 的应用场景分别是什么](https://github.com/Advanced-Frontend/Daily-Interview-Question/issues/249)
- [The Ultimate Guide to 3xx HTTP Status Codes](https://www.sitepoint.com/3xx-http-status-codes-ultimate-guide/)
