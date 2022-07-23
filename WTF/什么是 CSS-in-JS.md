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
- [Styled Components](https://styled-components.com/)

其中最流行的工具之一是 Styled Components。

这意味着它是 [CSS Modules](https://github.com/css-modules/css-modules) 的继承者，是一种编写 CSS 的方法，其作用范围仅限于单个组件，而不会泄漏到页面中的任何其他元素。

Styled Components 允许您在组件中编写简单的 CSS，而不用担心类名冲突。

> 推荐：[使用 Styled Components 编写样式化组件](https://github.com/lio-zero/blog/blob/main/React/%E4%BD%BF%E7%94%A8%20Styled%20Components%20%E7%BC%96%E5%86%99%E6%A0%B7%E5%BC%8F%E5%8C%96%E7%BB%84%E4%BB%B6.md)。

## 进一步阅读

- [What are CSS Modules and why do we need them?](https://css-tricks.com/css-modules-part-1-need/)
- [CSS Modules](https://glenmaddern.com/articles/css-modules)
