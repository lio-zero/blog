# 无框架 Web Components

## 什么是 Web Components？

> [Web Components](https://developer.mozilla.org/zh-CN/docs/Web/Web_Components) 是一套不同的技术，允许你创建可重用的定制元素（它们的功能封装在你的代码之外）并且在你的 web 应用中使用它们。不需要需要任何外部库来工作。

## 特征

- **Custom Elements**
- **Shadow DOM**
- **HTML Templates**
- **HTML Import** 允许导入的外部 HTML 文档。

## Web Components 是如何工作的

我们经常使用的 HTML5 `<video>` 和 `<audio>` 元素，它们允许用户使用一系列内部按钮和控件播放、暂停、回放和快进媒体。默认情况下，浏览器处理布局、样式和功能。

Web Components 允许你添加自己的 HTML 自定义元素。元素名称必须包含连字符（`-`），以确保它不会与正式 HTML 元素冲突。

然后为你的自定义元素注册一个 ES6 类。它可以附加 DOM 元素，比如按钮、标题、段落等。为了确保这些元素不会与页面的其余部分冲突，你可以将它们附加到具有自己作用域样式的 Shadow DOM 内部。你可以将其视为迷你版 `<iframe>`，尽管 CSS 属性（如字体和颜色）是通过级联继承的。

最后，你可以使用可复用的 HTML Templates 将内容附加到 Shadow DOM 中，HTML Templates 通过标签提供一些配置。

与框架相比，标准 Web Components 是最基本的。它们不包括数据绑定和状态管理等功能，但 Web Components 具有一些自身优势：

- 它们轻巧快速
- 它们可以实现单独用 JavaScript 无法实现的功能（例如 Shadow DOM）
- 它们可以在任何 JavaScript 框架内工作
- 它们将得到浏览器的支持，也就是新特性红利。

## 更多资料

- 推荐阮一峰老师的 [Web Components 入门实例教程](https://www.ruanyifeng.com/blog/2019/08/web_components.html)，通过一个示例帮助你如何使用 Web Component API 开发组件。
- 关于 Web Components 的讨论，可以查阅 [WEBCOMPONENTS](https://www.webcomponents.org/)，其中提供了很多 Web Components 及相关 Web 规范的讨论。
- [Web components](https://zh.javascript.info/web-components)
- [The broken promise of Web Components](https://dmitriid.com/blog/2017/03/the-broken-promise-of-web-components/)
- [Regarding the broken promise of Web Components](https://robdodson.me/posts/regarding-the-broken-promise-of-web-components/)
- [精读《Web Components 的困境》](https://github.com/ascoders/weekly/blob/master/%E5%89%8D%E6%B2%BF%E6%8A%80%E6%9C%AF/10.%E7%B2%BE%E8%AF%BB%E3%80%8AWeb%20Components%20%E7%9A%84%E5%9B%B0%E5%A2%83%E3%80%8B.md)
