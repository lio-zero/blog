# 什么是 Serverless？

Serverless 是一个术语，它标识了一种特定的运行程序的方式，不涉及管理你自己的服务器。

你创建一个函数，将它放在云服务器的某个位置，然后你所拥有的就是一个要调用的 URL。

当你调用该 URL 时，函数将被执行。

其他人管理服务器、扩展和安全。不需要担心内核更新或移动到你的 Linux 发行版的下一个 LTS 版本。

从定价模型来看，Serverless 也非常方便。传统上，你可能会每月租用一台 VPS（虚拟专用服务器），并按月支付费用，不管你的实际使用情况。

如果由于有人在受欢迎的地方分享了你的网站而导致用户激增，那么除非你升级到更大的服务器，否则服务器可能无法满足所有请求。

使用 Serverless，你需要为请求付费，而不是为服务器付费。如果没有人使用你的服务，你无需支付任何费用。如果有 10 万人一起跳转到你的网站，你的功能就会扩大，因为为你管理功能的公司已经具备了处理流量的所有要素，并自动将更多资源投入到你的功能中。你为使用的资源付费，而不是为将来可能使用的某些资源付费。

如果操作得当，从心理角度来看，这对独立开发项目的开发人员来说也是一种解放。你不负责为应用提供动力的服务器，因此你不会 24/7 全天候待命以修复可能发生的任何问题。你不必是一个系统管理员或 devops 专家来运行你的应用。

**听起来很梦幻，有哪些陷阱？**

首先，Serverless 仍处于早期的阶段。所有参与者都有不同的实现方式，围绕它的工具在质量上也各不相同。

在定价方面，如果你有一个可预测的流量，并且你可以以更低的价格购买服务器，例如通过在阿里云上预订实例，那么这对你可能没有意义。

你也不能控制服务器，这意味着你必须依赖可用的基础设施进行日志记录、监视和调试，而且很难在本地重新生成你的设置。

## 更多资料

以上仅粗略介绍了 Serverless，以下提供一些链接供大家学习：

- [frontend-focus/Serverless](https://github.com/lio-zero/frontend-focus#serverless) — 一份 Serverless 精彩列表
- [Serverless 初探](https://cloud.tencent.com/developer/article/1005537)
- [Serverless（无服务）基础知识](https://juejin.cn/post/6844903904224903181)
- [探索 Serverless 中的前端开发模式](https://juejin.cn/post/6844903844745330695)
- [你学 BFF 和 Serverless 了吗](https://juejin.cn/post/6844904185427673095)
- [Building an API using Serverless framework](https://awstip.com/building-an-api-using-serverless-framework-cbc1e8912806)
