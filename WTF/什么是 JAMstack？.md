# 什么是 JAMstack？

在过去的几年里，你肯定遇到过 [JAMstack](https://jamstack.org/) 这个术语。

它定义了一组用于实现目标的技术，如 [LAMP](https://zh.wikipedia.org/wiki/LAMP) 和 [MEAN](https://zh.wikipedia.org/wiki/MEAN)（如果您熟悉的话）。

## JAMstack 是什么意思？

JAM 代表 JavaScript、API 以及 Markup。而 stack 译为**技术栈**，也就是构建应用时具体使用到的技术集合。

它描述了创建具有以下关键特征的 Web 应用和网站的趋势：

- 有一个 "dumb" 的 Web 服务器（或 CDN）发送运行应用程序所需的 HTML，通常使用静态网站生成器生成。没有生成 HTML。
- 应用程序可以加载一些从 API 接收数据的 JavaScript。像导航这样的页面交互可能导致从 API 获取更多的数据。身份验证也是通过 API 完成的。

与传统方法相比，这种新方法是一种新的方式：

- 应用程序中已经提供内容的传统网站（如静态网站）
- 基于 CMS 的网站，从后端的数据库加载信息
- 使用任何后端语言的服务器渲染应用程序

它也不同于具有服务器端渲染部分的客户端渲染网站（例如，使用 React 构建）。JAMstack 根本不涉及服务器渲染。

## 使用 JAMstack 有什么优势？

- **它很快**。HTML 已经生成，Web 服务器必须只为它提供服务，而不涉及任何类型的后端操作，例如从数据库中查找数据或为每个请求生成页面 HTML。它可以很容易地通过 CDN（内容分发网络）提供服务。
- **它是有效的**。因为没有后端，所以没有瓶颈（例如，没有数据库）。
- **它更便宜**，因为通过 CDN 提供资源比通过后端服务器提供资源的成本要低得多
- **它更安全**，因为后端仅通过 API 公开

服务器动态渲染网站应用程序的传统方法，例如 WordPress、Laravel 和 Rails，在许多情况下正被更轻的方法所取代。

一个典型的 WordPress 网站可以为每个页面加载向数据库发出 30-100 个请求，具体取决于安装的插件数量。除非被大量缓存，否则当它在 [Hacker News](https://news.ycombinator.com/)、[Reddit](https://www.reddit.com/) 或任何其他大型网站上流行时，您都可以认出它，因为您会看到一个空白页面，这意味着服务器崩溃了，因为该网站无法支持所有流量。很多时候，这是一个失去的机会，因为当网站的人气达到顶峰时，它就不起作用了。

提供静态 HTML 页面要比这高效得多，而且在需要时仍然可以获取动态数据，在加载 HTML 后使用单独的 API 调用。

![JAMstack](https://upload-images.jianshu.io/upload_images/18281896-e5d5dcf792f97069.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 有哪些技术可用于构建 JAMstack 应用程序？

- [Gatsby](https://www.gatsbyjs.org/)
- [Next.js](https://flaviocopes.com/nextjs/)
- [Nuxt.js](https://nuxtjs.org/)
- [Hugo](https://gohugo.io/)
- etc

还有很多，您可以在 JAMstack 官网 [Site Generators](https://jamstack.org/generators/) 查找几乎所有很能的静态站点生成器列表。

另外，它还列出了众多的 [Headless CMS](https://jamstack.org/headless-cms/)。

## JAMstack 可以用来做什么？

许多应用程序都可以被归入 JAMstack 的范畴，其可能性是无限的，从简单的博客到电子商务网站，再到更复杂的 Web 应用程序。

## 更多资料

2021 年 Jamstack 社区调查可以查看 [Jamming into the Mainstream: Jamstack Community Survey 2021](https://jamstack.org/survey/2021/)。另外，2022 的调查正在进行中。

其他：

- [Jamstack Training](https://jamstack.training/) — Jamstack 视频课程
- [JAMSTACK](https://css-tricks.com/tag/jamstack/)
- 关于 Jamstack 的对话：[Demystifying JAMstack: An Interview With Phil Hawskworth](https://www.smashingmagazine.com/2019/05/demystifying-jamstack-interview-phil-hawskworth/) 和 [JAMstack Fundamentals: What, What And How](https://www.smashingmagazine.com/2019/06/jamstack-fundamentals-what-what-how/)
