# 无框架 Web 组件

## 什么是 Web 组件？

> [Web Component](https://developer.mozilla.org/zh-CN/docs/Web/Web_Components) 是一套不同的技术，允许您创建可重用的定制元素（它们的功能封装在您的代码之外）并且在您的 web 应用中使用它们。不需要需要任何外部库来工作。

## 特征

- **Custom elements（自定义元素）**
- **Shadow DOM（影子 DOM）**
- **HTML templates（HTML 模板）**
- **HTML Import** 允许导入的外部 HTML 文档。

## Web 组件是如何工作的

我们经常使用的 HTML5 `<video>` 和 `<audio>` 元素，它们允许用户使用一系列内部按钮和控件播放、暂停、回放和快进媒体。默认情况下，浏览器处理布局、样式和功能。

Web 组件允许您添加自己的 HTML 自定义元素。元素名称必须包含连字符（`-`），以确保它不会与正式 HTML 元素冲突。

然后为您的自定义元素注册一个 ES6 类（`class`）。它可以附加 DOM 元素，如按钮、标题、段落等。为了确保这些元素不会与页面的其余部分冲突，您可以将它们附加到具有自己范围样式的内部 **Shadow DOM**。您可以将其视为迷你版 `<iframe>`，尽管 CSS 属性（如字体和颜色）是通过级联继承的。

最后，您可以使用可重用的 HTML 模板将内容附加到 **Shadow DOM** 中，HTML 模板通过标签提供一些配置。

与框架相比，标准 web 组件是最基本的。它们不包括数据绑定和状态管理等功能，但 web 组件具有一些自身优势：

- 它们轻巧快速
- 它们可以实现单独用 JavaScript 无法实现的功能（例如 Shadow DOM）
- 它们可以在任何 JavaScript 框架内工作
- 它们将得到浏览器的支持，也就是新特性红利。

## 更多资料

- 推荐阮一峰老师的 [Web 组件入门实例教程](http://www.ruanyifeng.com/blog/2019/08/web_components.html)，通过一个示例帮助您如何使用 Web Component API 开发组件。
- 关于 Web 组件的讨论，可以查阅 [WEBCOMPONENTS](https://www.webcomponents.org/)，其中提供了很多 Web 组件及相关 Web 规范的讨论。
- [Web components](https://zh.javascript.info/web-components)
- [The broken promise of Web Components](https://dmitriid.com/blog/2017/03/the-broken-promise-of-web-components/)
- [Regarding the broken promise of Web Components](https://robdodson.me/posts/regarding-the-broken-promise-of-web-components/)
