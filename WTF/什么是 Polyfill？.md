# 什么是 Polyfill？

[Polyfill](https://developer.mozilla.org/zh-CN/docs/Glossary/Polyfill) 是一块代码（通常是 Web 上的 JavaScript），用来为旧浏览器提供它没有原生支持的较新的功能。

例如：浏览器不支持[管道运算符](https://github.com/lio-zero/blog/blob/main/JavaScript/%E5%A6%82%E4%BD%95%E5%9C%A8%20JavaScript%20%E4%B8%AD%E4%BD%BF%E7%94%A8%E7%AE%A1%E9%81%93%EF%BC%88%E7%AE%A1%E9%81%93%E8%BF%90%E7%AE%97%E7%AC%A6%EF%BC%89%EF%BC%9F.md)，Babel 提供了 [`@babel/plugin-proposal-pipeline-operator`](https://babeljs.io/docs/en/babel-plugin-proposal-pipeline-operator) 插件可以让我们在浏览器中使用它。

一些不错的 Polyfill：

- [fetch](https://github.com/github/fetch) `window.fetch` JavaScript polyfill。
- [fastclick](https://github.com/ftlabs/fastclick) 可消除带有触摸 UI 的浏览器上的点击延迟 Polyfill。另外，关于移动设备浏览器 300ms 点击延迟 有不清楚的，可以阅读[移动端 300ms 点击延迟和点击穿透](https://juejin.cn/post/6844903633528553485)。
- [Respond](https://github.com/scottjehl/Respond) 用于最小/最大宽度 CSS3 媒体查询的快速轻量级 polyfill（适用于 IE 6-8 等）
- [es6-promise](https://github.com/stefanpenner/es6-promise) ES6 风格的 Promise 的 polyfill
- [unfetch](https://github.com/developit/unfetch) 🐕 最小 500b 的 `fetch` polyfill。

Modernizr 提供了 HTML5 特性所有可能的 polyfill 列表：[HTML5 Cross Browser Polyfills](https://github.com/Modernizr/Modernizr/wiki/HTML5-Cross-browser-Polyfills)。

[Polyfill.io](https://polyfill.io/v3/) 是一种通过选择性地填充浏览器所需的内容来减少 Web 开发挫折的服务。Polyfill.io 读取每个请求的 User-Agent header，并返回适用于请求浏览器的 polyfill。

> **Tips**：还是有必要说明一下，上面这些 Polyfill 随着 Web 的发展，大多数原生功能在浏览器中都已被很好的支持。您可能不在需要它们，这要看您是否需要兼容低版本浏览器。另外，可以在 [Can I Use](https://caniuse.com/) 上查看它们的支持情况。

## 更多资料

[What is a Polyfill?](https://remysharp.com/2010/10/08/what-is-a-polyfill)（Remy Sharp，概念发明者）
