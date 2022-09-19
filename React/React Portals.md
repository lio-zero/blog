# React Portals

React 16 引入了 [Portal](https://reactjs.org/docs/portals.html)。

Portal 是一种将元素渲染在其组件层次结构之外的单独组件中的一种方式。

当该事件被渲染时，发生在其上的事件由 React 组件层次结构管理，而不是由元素的 DOM 位置设置的层次结构管理。

因此命名为 **Portal** — 一个元素位于 DOM 树中的某个位置，它位于正常的 React 组件树之外，但包含它的 React 组件树仍然对它负责。

React 提供了一个简单的 API 来实现这一点，即 `ReactDOM.createPortal()`，它接受 2 个参数。第一个是要渲染的元素，第二个是要渲染它的 DOM 元素。

```js
ReactDOM.createPortal(child, container)
```

Portal 的一个典型的用例是模态窗口。

全屏渲染的模态必须位于元素之外，因此可以使用 CSS 正确设置样式。

这是 React 官网提供的一个示例：[Example: Portal event bubbling](https://codepen.io/gaearon/pen/jGBWpE)。

> **扩展**：Vue 3 中也实现了一个类似的内置组件 Teleport。感兴趣的话可以阅读 [Vue3 Teleport 组件](https://github.com/lio-zero/blog/blob/main/Vue/Vue3%20Teleport%20%E7%BB%84%E4%BB%B6.md)。
