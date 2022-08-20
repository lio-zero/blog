# React 纯组件

在 JavaScript 中，当一个函数不改变对象而只是返回一个新对象时，它被称为[纯函数](https://github.com/lio-zero/blog/blob/main/JavaScript/JavaScript%20%E7%BA%AF%E5%87%BD%E6%95%B0.md)。

为了被称为纯函数，函数或方法不应产生副作用，并且在使用相同的输入多次调用时应返回相同的输出。

纯函数接受输入并返回输出，而不更改输入或其他任何内容。

其输出仅由参数决定。您可以调用此函数无数次，并且给定相同的参数，输出将始终相同。

React 将这个概念应用于组件。当 React 组件的输出仅依赖于它的 `props` 时，它就是一个纯组件。

所有功能组件都是纯组件：

```js
const Button = (props) => {
  return <button>{props.message}</button>
}
```

如果类组件的输出仅依赖于 `props`，则它们可以是纯的：

```js
class Button extends React.Component {
  render() {
    return <button>{this.props.message}</button>
  }
}
```

这里需要注意一下 [`React.PureComponent`](https://reactjs.org/docs/react-api.html#reactpurecomponent) 和 `React.Component` 的区别。

## 更多资料

[7 Architectural Attributes of a Reliable React Component](https://dmitripavlutin.com/7-architectural-attributes-of-a-reliable-react-component/)
