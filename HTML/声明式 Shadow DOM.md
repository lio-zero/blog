# 声明式 Shadow DOM

Shadow DOM 在 Web 组件的世界中扮演着重要的角色，因为它们提供了一种简单的封装方式，但它是使用 JavaScript 强制创建的，具有以下特性：

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

嗯，这对仅客户端的应用程序很有效，但是在服务器（SSR）上表达和构建它们有很多挑战，当声明性阴影出现时！

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
