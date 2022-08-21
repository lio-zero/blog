# React 高阶组件

您可能熟悉 JavaScript 中的[高阶函数](https://github.com/lio-zero/blog/blob/main/JavaScript/JavaScript%20%E9%AB%98%E9%98%B6%E5%87%BD%E6%95%B0.md)。它是将函数作为参数或将函数作为结果返回的函数。

在 React 中，我们将这个概念扩展到组件，因此当组件接受组件作为输入并返回组件作为输出时，我们就有了一个高阶组件（HOC）。

通常，高阶组件允许您创建可组合和可复用的代码，并且更具封装性。

我们可以使用 HOC 将方法或属性添加到组件的状态，例如 [Redux](https://redux.js.org/) 存储。

当您想要增强现有组件、操作状态或 `props` 或其渲染的标签时，可能就需要使用高阶组件。

有一个约定是用 `with` 前缀预先设置高阶组件（这是一个约定，所以它不是强制性的），因此如果您有一个 `Button` 组件，它的 HOC 对应项应该被称为 `withButton`。

让我们创建一个。

最简单的 HOC 示例是返回未更改的组件：

```js
const withElement = (Element) => () => <Element />
```

让我们让它变得更有用一点，除了它已经附带的所有 `props`，我们为该元素添加一个 `size` 属性：

```js
const withGreet = (Element) => (props) => <Element {...props} size='small' />
```

我们在组件 JSX 中使用这个 HOC：

```js
const Button = (props) => (
  <button>
    Hello {props.value} {props.size}
  </button>
)

const GreetedButton = withGreet(Button)
```

最后，在 App JSX 中渲染 `GreetedButton` 组件：

```js
function App() {
  return (
    <div className='App'>
      <h1>Hello</h1>
      <GreetedButton />
    </div>
  )
}
```

这是一个非常简单的示例，但希望在将这些概念应用于更复杂的场景之前，您能够了解 HOC 的要点。

## 更多资料

高阶组件（HOC）是 React 中用于复用组件逻辑的一种高级技巧。HOC 自身不是 React API 的一部分，它是一种基于 React 的组合特性而形成的设计模式。

具体而言，高阶组件是参数为组件，返回值为新组件的函数。

- [Docs：Higher-Order Components](https://reactjs.org/docs/higher-order-components.html)
- [React 高阶组件(HOC)入门指南](https://github.com/MrErHu/blog/issues/4)
- [hoc 库 recompose](https://github.com/acdlite/recompose) 函数组件和高阶部件的 React 实用皮带。
- [精读 React 高阶组件](https://zhuanlan.zhihu.com/p/27434557)
