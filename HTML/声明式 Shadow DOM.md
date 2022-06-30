# 声明式 Shadow DOM

[Shadow DOM](https://developer.mozilla.org/zh-CN/docs/Web/Web_Components/Using_shadow_DOM) 在 Web 组件的世界中扮演着重要的角色，因为它们提供了一种简单的封装方式，但它是使用 JavaScript 强制创建的，具有以下特性：

```js
class FancyElement extends HTMLElement {
  constructor() {
    super()
    this.ShadowRoot = this.attachShadow({ mode: 'open' })
    this.ShadowRoot.innerHTML = 'Shadow DOM imperative'
  }
}

customElements.get('imperative-fancy-element') ||
  customElements.define('imperative-fancy-element', FancyElement)
```

这仅对客户端的应用程序有效，但是在服务器渲染（SSR）上表达和构建它们有很多挑战，声明式 Shadow 出现帮我们解决这个问题！

声明式 Shadow DOM（DSD）帮助我们轻松地将 Shadow DOM 发送到服务器，上面的示例命令式示例如下所示：

```html
<imperative-fancy-element>
  <h1>Imperative Shadow DOM</h1>
</imperative-fancy-element>

<fancy-element>
  <template shadowroot="open">
    <slot></slot>
  </template>
  <h2>Declarative Shadow DOM</h2>
</fancy-element>
```

> **注意**：`chrome://flags/#enable-experimental-web-platform-features` 应在浏览器启用才能使用此功能。

## 更多资料

它是 Web components 的一部分，推荐阅读[无框架 Web 组件](https://github.com/lio-zero/blog/blob/main/JavaScript/%E6%97%A0%E6%A1%86%E6%9E%B6%20Web%20%E7%BB%84%E4%BB%B6.md)。
