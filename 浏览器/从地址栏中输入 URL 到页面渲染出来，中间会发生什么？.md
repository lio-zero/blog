# 从地址栏中输入 URL 到页面渲染出来，中间会发生什么？

GitHub 上有一个仓库 [what-happens-when](https://github.com/alex/what-happens-when)，它尝试回答一个古老的面试问题：**当你在浏览器中输入 google.com 并按下 Enter 键后会发生什么？**

它不再局限于平常的回答，而是更具体详细的解读，不遗漏任何细节。

一个典型的现代浏览器需要经历一系列步骤，从你在地址栏中输入的 URL 到最终在屏幕上看到的页面。它必须经过：

1. 解析 DNS
2. 建立与服务器的 TCP 连接
3. 请求页面上的所有资源（也就是发送 HTTP 请求，服务器处理 HTTP 请求并返回报文）
4. 构建 DOM 和 CSSOM
5. 构建渲染树
6. 执行布局
7. 解码图像
8. 在屏幕上绘制

我们可以将以上步骤分为两个常规阶段：

- 步骤 1 - 3 **受网络限制**。它们发生的速度和成本主要取决于网络的特性：带宽、延迟、数据成本等。
- 步骤 4 - 8 **受设备约束**。这些步骤发生的速度主要取决于设备和浏览器的特性：处理器，内存等。

另外，步骤 4 - 8 是浏览器解析渲染页面的过程。

我不会再这介绍它们每一个步骤都具体做了什么，因为有人 👇 已经做出了总结。

> 这也是一道经典的面试题。回答时，按上面这些步骤，由浅入深，逐步深入分析。

## 更多资料

我整理了一些资源，供大家参考：

- [what-happens-when](https://github.com/alex/what-happens-when)
- [how-web-works](https://github.com/vasanthk/how-web-works)
- [细说浏览器输入 URL 后发生了什么](https://juejin.cn/post/6844904054074654728)
- [说一说从输入 URL 到页面呈现发生了什么？](https://juejin.cn/post/6844904021308735502#heading-24)
- [图解浏览器渲染原理及流程](https://mp.weixin.qq.com/s?__biz=MzU2MTIyNDUwMA==&mid=2247507400&idx=1&sn=5b02305919bb564fef121551d41e59f8&chksm=fc7e9393cb091a85f65b05fd710bac00b221b7e9fac31e836b8334a008bafe9da0e65b61cb9d&scene=178&cur_album_id=2120079708137586688#rd)
