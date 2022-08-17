# JavaScript V8 引擎

[V8](https://github.com/v8/v8) 是支持 Google Chrome 的 JavaScript 引擎的名称。它使用我们的 JavaScript，并在使用 Chrome 浏览器浏览时执行它的东西。

V8 提供了 JavaScript 执行的运行时环境。DOM 和其他 Web 平台 API 由浏览器提供。

很酷的一点是 JavaScript 引擎是独立于它所在的浏览器的。这个关键特性促成了 Node.js 的兴起。早在 2009 年，V8 就被选为支持 Node.js 的引擎，随着 Node.js 的流行，V8 成为了现在支持大量 JavaScript 编写的服务器端代码的引擎。

Node.js 生态系统非常庞大，多亏了它，V8 也支持桌面应用程序，比如 Electron 等项目。

## 其他 JS 引擎

其他浏览器都有自己的 JavaScript 引擎:

- Firefox 有 Spidermonkey
- Safari 有 JavaScriptCore（也称为 Nitro）
- Edge 有 Chakra
- 还有很多其他的

您可以在我的另一篇文章[浏览器内核](https://github.com/lio-zero/blog/blob/main/%E6%B5%8F%E8%A7%88%E5%99%A8/%E6%B5%8F%E8%A7%88%E5%99%A8%E5%86%85%E6%A0%B8.md)。

所有这些引擎都实现了 ECMA ES-262 标准，也称为 ECMAScript，这是 JavaScript 使用的标准。

## 对性能的追求

V8 是用 C++ 编写的，并且还在不断改进。它是可移植的，可以在 Mac、Windows、Linux 和其他一些系统上运行。

在本文 V8 的介绍中，我将忽略 V8 的实现细节：它们可以在更权威的网站上找到（例如 V8 的官网），并且它们会随着时间的推移而变化，而且通常会发生根本性的变化。

与其他 JavaScript 引擎一样，V8 总是在不断发展，以加快 Web 和 Node.js 生态系统。

在 Web 上，多年来一直存在着一场性能竞赛，我们（作为用户和开发人员）从这场竞争中受益良多，因为我们年复一年地获得更快和更优化的机器。

## 汇编

JavaScript 通常被认为是一种解释型语言，但现代 JavaScript 引擎不再只是解释 JavaScript，而是编译它。

自从 2009 年 SpiderMonkey JavaScript 编译器被添加到 Firefox 3.5 之后，每个人都遵循这个想法。

JavaScript 由 V8 内部编译，采用即时（JIT）编译来加快执行速度。

这看起来似乎有悖直觉，但自从 2004 年引入谷歌地图以来，JavaScript 已经从一种通常执行几十行代码的语言发展为在浏览器中运行数千到数十万行代码的完整应用程序。

我们的应用程序现在可以在浏览器中运行数小时，而不仅仅是一些表单验证规则或简单的脚本。

编译 JavaScript 非常有意义，因为尽管准备好 JavaScript 可能需要更多的时间，但一旦完成，它的性能将比纯解释代码的性能好得多。

## 更多资料

- [Vyacheslav Egorov](https://mrale.ph/) 是一名 V8 引擎工程师，您可以关注它的博客。
- [V8 博客](http://v8project.blogspot.com/)
- [V8 是怎么跑起来的 —— V8 的 JavaScript 执行管道](https://juejin.cn/post/6844903990073753613)
- [V8 编译浅谈](https://juejin.cn/post/7041021350114230285)
- [认识 V8 引擎](https://zhuanlan.zhihu.com/p/27628685)
