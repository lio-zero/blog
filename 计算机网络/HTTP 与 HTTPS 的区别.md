# HTTP 与 HTTPS 的区别

HTTP（超文本传输协议）是为我们所知的网络提供支持的协议。

它位于 TCP 之上，TCP 位于 IP 之上。

网页可以使用 HTTP 或 HTTPS（安全超文本传输协议）。

**它们有何不同？**而且，为什么现在 HTTP 被 Chrome 标记为不安全？

## 安全

当您从服务器请求 HTTP 页面时，数据会通过许多不同的网络，每个网络由单独的公司或实体控制。

从可能属于咖啡店或城市公共网络基础设施的 Wi-Fi 路由器开始，网络中的每个节点都可以看到请求和响应，并以任何方式对其进行修改。

他们可能会注入广告、恶意软件，并记录您输入的任何凭据。中间的服务器可以充当中间人，发送受损信息。

这也适用于任何不安全的互联网协议。

HTTPS 流量是端到端加密的，这意味着两者之间没有任何东西可以读取您与网络另一端服务器之间交换的信息。

现代的浏览器会将 HTTP 站点标记为不安全。

## 端口

默认情况下，HTTP 在端口 80 上提供服务，而 HTTPS 在端口 443 上提供服务。这些是默认端口，但 Web 服务器可以选择在不同的随机端口上提供内容，在这种情况下，您需要在地址栏：

```bash
http://github.com/lio-zero
http://github.com:80/lio-zero
https://github.com:443/lio-zero
https://github.com:8081/lio-zero # ❌ 可以在不同的端口运行，但这里提供地址行不通
```

## HTTPS 会影响速度？

很多人人认为 HTTPS 所需的 TLS 握手使页面速度变慢，但实际上，HTTPS 页面的加载方式，比 HTTP 快得多。

**为什么？**因为 HTTP/2。HTTP/2 可以并行处理请求，并且需要安全连接，因此，如果您的服务器使用支持 HTTP/2 的现代 Web 服务器，那么在使用 HTTPS 时，您的网页将会有明显的速度提升。

HTTP/2 引入了更好的并行性、二进制传输、多路复用和 header 压缩等，这是对 HTTP 的一次了不起的更新。

> HTTP/3 已在六月份已被采纳为 IETF 标准。但应用范围还不足。后面我们会单独说一说。

您可以查看 [HTTP vs HTTPS Test](https://www.httpvshttps.com/) 和 [I wanna go fast: HTTPS' massive speed advantage](https://www.troyhunt.com/i-wanna-go-fast-https-massive-speed-advantage/)，以了解它们在速度上的对比。

## HTTPS 很难实现吗？

并不。开启 HTTPS 需要提供 SSL 证书，它用于网站的身份认证，可以确定网站真实性，并保证网站用户信息传输的机密性。

很多厂商都提供了免费的共享 SSL 证书，如果需要单独注册网络资源，也可以购买收费的。

您可以阅读 [The Complete Guide To Switching From HTTP To HTTPS](https://www.smashingmagazine.com/2017/06/guide-switching-http-https/) 了解如何从 HTTP 切换到 HTTPS 的具体过程。

至于免费证书，您可以购买国内的，例如阿里云提供的免费证书。冴羽有一篇 VuePress 搭建博客系列有一篇：[VuePress 博客优化之开启 HTTPS](https://github.com/mqyqingfeng/Blog/issues/246)，可以看看。

## 更多资料

- [HTTP vs HTTPS — What's the Difference?](https://www.freecodecamp.org/news/http-vs-https/)
- [HTTP 和 HTTPS 详解](https://juejin.cn/post/6844903604868874247)
- [20 分钟助你拿下 HTTP 和 HTTPS，巩固你的 HTTP 知识体系](https://juejin.cn/post/6994629873985650696)
- [What is HTTPS?](https://www.cloudflare.com/en-gb/learning/ssl/what-is-https/)
