# React Context API

引入 Context API 是为了允许您在应用程序中传递状态（并允许状态更新），而无需使用 `props`。

React 团队建议，如果你只有几个级别的子级需要传递，那么就坚持使用 `props`，因为它仍然是一个比 Context API 简单得多的 API。

在许多情况下，它使我们能够避免使用 Redux，大大简化我们的应用程序。

## 如何工作？

您使用 [`React.createContext()`](https://zh-hans.reactjs.org/docs/context.html#reactcreatecontext) 创建一个上下文，它返回一个 Context 对象：

```jsx
const { Provider, Consumer } = React.createContext()
```

然后创建一个返回 `Provider` 组件的包装器组件，并将所有要从中访问上下文的组件添加为子组件：

```jsx
function Container(props) {
  let [state, useState] = useState('Show')

  return <Provider value={state}>{props.children}</Provider>
}

function App() {
  return (
    <>
      <Container>
        <Button />
      </Container>
    </>
  )
}
```

我使用 `Container` 作为这个组件的名称，因为这将是一个全局 `provider`。

在包装在 `Provider` 中的组件内部，您可以使用 `Consumer` 组件来使用上下文：

```jsx
function Button() {
  return <Consumer>{(context) => <button>{context.state}</button>}</Consumer>
}
```

您还可以将函数传递给 `Provider` 值，`Consumer` 将使用这些函数来更新上下文状态：

```jsx
<Provider value={{ state, setState }}>{props.children}</Provider>

// ...
<Consumer>
  {({ state, setState }) => (
    <button onClick={() => setState('Hide')}>{state}</button>
  )}
</Consumer>
```

配合 Hooks：

```jsx
import { useContext } from 'react'

function Button() {
  const { state, setState } = useContext(UserContext)
  return <button onClick={() => setState('Hide')}>{state}</button>
}
```

您可以创建多个上下文，以使您的状态分布在各个组件之间，同时公开它并使其可以被您想要的任何组件访问。

使用多个文件时，可以在一个文件中创建内容，并将其导入到所有使用位置：

```jsx
// context.js
import React from 'react'
export default React.createContext()

// component1.js
import Context from './context'
// ... 使用 Context.Provider

// component2.js
import Context from './context'
// ... 使用 Context.Consumer
```
