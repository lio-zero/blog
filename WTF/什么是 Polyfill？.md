# 什么是 Polyfill？

[Polyfill](https://developer.mozilla.org/zh-CN/docs/Glossary/Polyfill) 是一块代码（通常是 Web 上的 JavaScript），用来为旧浏览器提供它没有原生支持的较新的功能。

例如：浏览器不支持[管道运算符](https://github.com/lio-zero/blog/blob/main/JavaScript/%E5%A6%82%E4%BD%95%E5%9C%A8%20JavaScript%20%E4%B8%AD%E4%BD%BF%E7%94%A8%E7%AE%A1%E9%81%93%EF%BC%88%E7%AE%A1%E9%81%93%E8%BF%90%E7%AE%97%E7%AC%A6%EF%BC%89%EF%BC%9F.md)，Babel 提供了 [`@babel/plugin-proposal-pipeline-operator`](https://babeljs.io/docs/en/babel-plugin-proposal-pipeline-operator) 插件可以让我们在浏览器中使用它。

一个不错的 [HTML5 Cross Browser Polyfills](https://github.com/Modernizr/Modernizr/wiki/HTML5-Cross-browser-Polyfills) 列表。

## 更多资料

[What is a Polyfill?](https://remysharp.com/2010/10/08/what-is-a-polyfill)（Remy Sharp，概念发明者）
