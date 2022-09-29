# HTTPS

HTTP（超文本传输协议）在设计上是不安全的。

当您打开浏览器并要求 web 服务器向您发送网页时，您的数据会执行 2 次行程：1 次从浏览器到 web 服务器，1 次从 web 服务器到浏览器。

然后，根据网页的内容，您可能需要更多的连接来获取 CSS 文件、JavaScript 文件、图像等。

在任何这些连接过程中，都可以检查和操作您的数据将要通过的任何网络。

这样做的后果可能很严重：您可能会让第三方监视和记录您的所有网络活动，您甚至不知道它的存在，某些网络可能会注入广告，您可能会受到中间人攻击，这是一种安全威胁，攻击者可以在网络上操纵您的数据，甚至模拟您的计算机。对于某些人来说，只需收听通过公共且未加密的 Wi-Fi 网络传输的 HTTP 数据包是非常容易的。

HTTPS（安全超文本传输协议）旨在从根本上解决这个问题：浏览器和 web 服务器之间的整个通信都是加密的。

隐私和安全是当今互联网的主要关注点。几年前，您只需在受登录保护的页面中使用加密连接。此外，由于 SSL 证书的定价和复杂性，大多数网站只使用 HTTP。

如今，HTTPS 在任何网站上都是必需的。现代的主流浏览器都将 HTTP 站点标记为不安全，只是为了给你一个有效的理由，在你的所有网站上强制使用 HTTPS。

使用 HTTP 时，默认服务器端口为 80，在 HTTPS 上为 443。当然，如果服务器使用默认端口，则无需显式添加。

HTTPS 有时也称为 HTTP over SSL，或 HTTP over TLS。两者的区别很简单：TLS 是 SSL 的继承者。

使用 HTTPS 时，唯一未加密的是 web 服务器域和服务器端口。

其他所有信息，包括资源路径、header、cookie 和查询参数都已加密。

这里不会详细分析 TLS 协议背后是如何工作的，但你可能会认为它增加了大量开销，你是对的。

添加到网络资源处理中的任何计算都会导致客户端、服务器和传输数据包大小的开销。

然而 HTTPS 支持使用 HTTP/2，它比 HTTP/1.1 有一个巨大的优势：它的速度更快。

**为什么？**原因有很多，一个是 header 压缩，一个是资源多路复用。还有一个是服务器推送：当请求一个资源时，服务器可以推送更多资源。因此，如果浏览器请求一个页面，它也会收到所需的所有资源（图像、CSS、JS）。

撇开细节不谈，HTTP/2 是对 HTTP/1.1 的巨大改进，它需要 HTTPS。这意味着，尽管 HTTPS 有加密开销，但如果使用现代设置正确配置，它的速度恰好要比 HTTP 快得多。

> HTTP 3 已在六月份已被采纳为 IETF 标准。

## 总结

HTTPS 是一种在 web 服务器和浏览器之间发送数据的安全方式。

- HTTPS（安全超文本传输协议）是 HTTP 的安全版本
- HTTPS 的默认端口为 443
- HTTPS 是基于 TLS/SSL 证书的传输，它只是在 HTTP 的基础上增加了加密
- 可以使用免费的共享 SSL 证书，如果需要单独注册网络资源，可以购买收费的，然后使用如 nginx 服务器进行配置。

## 更多资料

- [完全吃透 TLS/SSL](https://juejin.cn/post/6844903624640823310)
- [「知识拾遗」你应该知道的 https](https://mp.weixin.qq.com/s/aMYp6Y5n26r9vdQIom4g0w)
- [HTTPS 为何重要](https://developers.google.com/web/fundamentals/security/encrypt-in-transit/why-https)
- [Enabling HTTPS on Your Servers](https://developers.google.com/web/fundamentals/security/encrypt-in-transit/enable-https)
- [The Complete Guide To Switching From HTTP To HTTPS](https://www.smashingmagazine.com/2017/06/guide-switching-http-https/)
- [谈谈 HTTPS](https://juejin.cn/post/6844903504046211079)
