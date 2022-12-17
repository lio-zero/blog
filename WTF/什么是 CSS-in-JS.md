# 什么是 CSS-in-JS？

以前，Web 非常简单，CSS 甚至不存在。我们用 `table` 和 `frame` 布置页面。

然后 CSS 开始出现，一段时间后，框架可以极大地帮助构建网格和布局，[Bootstrap](https://getbootstrap.com/) 和 [Foundation](https://get.foundation/) 在这方面发挥了很大的作用。

诸如 SASS 和其他预处理器在很大程度上减缓了框架的采用，并更好地组织代码，BEM 和 SMACSS 等约定在使用中得到了发展，尤其是在团队中。

> 推荐：[BEM 命令规范](https://github.com/lio-zero/blog/blob/main/CSS/BEM%20%E5%91%BD%E4%BB%A4%E8%A7%84%E8%8C%83.md) 和 [SASS 预处理器](https://github.com/lio-zero/blog/blob/main/CSS/SASS%20%E9%A2%84%E5%A4%84%E7%90%86%E5%99%A8.md)

约定并不是万能的解决方案，而且它们很难记住，因此在过去几年中，随着 JavaScript 和构建过程在每个前端项目中的使用越来越多，CSS 逐渐融入 JavaScript（JS-in-CSS）。

新工具探索了使用 CSS-in-JS 的新方法，其中一些工具取得了成功，并越来越受欢迎：

- [Styled JSX](https://github.com/vercel/styled-jsx)
- [jsxstyle](https://github.com/jsxstyle/jsxstyle)
- [Radium](https://github.com/FormidableLabs/radium)
- [Emotion](https://github.com/emotion-js/emotion)
- [Styled Components](https://www.styled-components.com/)
- [JSS](http://cssinjs.org/)
- [Aphrodite](https://github.com/Khan/aphrodite)
- etc

你可以在 [css-in-js](https://github.com/MicheleBertoli/css-in-js) 这个仓库中找到这些库，这是一份众多 CSS in JS 技术的比较。

其中最流行的工具之一是 Styled Components 和 Emotion。

这意味着它是 [CSS Modules](https://github.com/css-modules/css-modules) 的继承者，是一种编写 CSS 的方法，其作用范围仅限于单个组件，而不会泄漏到页面中的任何其他元素。

Styled Components 允许你在组件中编写简单的 CSS，而不用担心类名冲突。

> 推荐：[使用 Styled Components 编写样式化组件](https://github.com/lio-zero/blog/blob/main/React/%E4%BD%BF%E7%94%A8%20Styled%20Components%20%E7%BC%96%E5%86%99%E6%A0%B7%E5%BC%8F%E5%8C%96%E7%BB%84%E4%BB%B6.md)。

使用 CSS-in-JS 的好处：

- 组件化思考 — 无需维护一堆样式表，样式直接在组件上编写。
- 消除无用代码 — 当组件被渲染时，样式被应用，组件销毁时，样式随组件消失。
- 范围界定 — 编写新样式不会影响到网站的其他内容，无需担心全局范围的样式受到影响。
- 命名 — 无需考虑设置一个 ID 或类。
- CSS 中 JavaScript 的可能性 — 在样式中使用 JS 特有的功能。
- 代码共享 — 往往一个组件设计好后，可以在任何项目中使用。
- 设计系统友好

使用 CSS-in-JS 的缺点：

- 学习曲线
- 新的依赖
- 挑战现状

有一个不错的 [CSS-in-JS playground](https://www.cssinjsplayground.com/)，你可以在这里尝试各种流行的 CSS-in-JS 解决方案，并支持实时预览。

## 进一步阅读

- [What are CSS Modules and why do we need them?](https://css-tricks.com/css-modules-part-1-need/)
- [CSS Modules](https://glenmaddern.com/articles/css-modules)
- [awesome-css-in-js](https://github.com/tuchk4/awesome-css-in-js)
- [CSS in JS: Benefits, Drawbacks, and Tooling](https://medium.com/object-partners/css-in-js-benefits-drawback-and-tooling-80286b03f9aa)
- [Facebook 重构：抛弃 Sass / Less，迎接原子化 CSS 时代](https://juejin.cn/post/6917073600474415117)
- [重新构想原子化 CSS](https://antfu.me/posts/reimagine-atomic-css-zh)
- [Why We're Breaking Up with CSS-in-JS](https://dev.to/srmagura/why-were-breaking-up-wiht-css-in-js-4g9b) — Emotion 的维护者 Sam 所在公司弃用了 CSS-in-JS 方案，引起了不小的讨论。译文可看 [「译」为什么我们正在放弃 CSS-in-JS](https://juejin.cn/post/7158712727538499598)
- [精读《请停止 css-in-js 的行为》](https://github.com/ascoders/weekly/blob/master/%E5%89%8D%E6%B2%BF%E6%8A%80%E6%9C%AF/7.%E7%B2%BE%E8%AF%BB%E3%80%8A%E8%AF%B7%E5%81%9C%E6%AD%A2%20css-in-js%20%E7%9A%84%E8%A1%8C%E4%B8%BA%E3%80%8B.md)
