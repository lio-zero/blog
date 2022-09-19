# React 中的代码拆分

就包大小而言，现代 JavaScript 应用可能相当庞大。您不希望您的用户仅仅为了加载首屏就必须下载一个 1MB 甚至更大的 JavaScript 包，对吗？但是，当您发布一个使用 Webpack 捆绑构建的现代 Web 应用时，默认情况下会发生这种情况。

捆绑包将包含可能永远不会运行的代码，因为用户可能只会在登录页面上停止，而永远不会看到应用的其余部分。

**代码拆分**是只在需要的时候加载 JavaScript 的做法。

**代码拆分**有两种形式，基于路由的拆分与基于组件的拆分，[react-loadable](https://github.com/thejameskyle/react-loadable) 库的 [Route-based splitting vs. Component-based splitting](https://github.com/jamiebuilds/react-loadable#route-based-splitting-vs-component-based-splitting) 这一小节有一个好的解释。

这改善了：

- 应用的性能
- 对内存的影响，以及移动设备上的电池使用情况
- 下载包的大小

React 16.6 引入了 `React.lazy` 和 `React.Suspense` 方法来执行代码拆分，它们取代以前使用的所有工具或库，例如：[react-loadable](https://github.com/thejameskyle/react-loadable)。

[`React.lazy`](https://zh-hans.reactjs.org/docs/code-splitting.html#reactlazy) 和 [`React.Suspense`](https://zh-hans.reactjs.org/docs/react-api.html#reactsuspense) 形成延迟加载依赖项并仅在需要时加载它的完美方式。

让我们从 `React.lazy` 开始。您可以使用它来导入任何组件：

```js
import React from 'react'

const TodoList = React.lazy(() => import('./TodoList'))

export default () => {
  return (
    <>
      <TodoList />
    </>
  )
}
```

`TodoList` 组件一旦可用，就会动态添加到输出中。Webpack 将为其创建一个单独的包，并在必要时负责加载。

`Suspense` 是一个组件，可用于包装任何延迟加载的组件：

```js
import React from 'react'

const TodoList = React.lazy(() => import('./TodoList'))

function App() {
  return (
    <>
      <React.Suspense>
        <TodoList />
      </React.Suspense>
    </>
  )
}
```

它负责在获取和渲染延迟加载的组件时处理输出。

使用它的 `fallback` 属性输出一些 JSX 或组件：

```js
// ...
<React.Suspense fallback={<p>Loading...</p>}>
  <TodoList />
</React.Suspense>
// ...
```

所有这些都与 React Router 配合良好：

```js
import React from 'react'
import { BrowserRouter as Router, Route, Switch } from 'react-router-dom'

const TodoList = React.lazy(() => import('./TodoList'))
const NewTodo = React.lazy(() => import('./NewTodo'))

const App = () => (
  <Router>
    <React.Suspense fallback={<p>Loading...</p>}>
      <Switch>
        <Route exact path='/' component={TodoList} />
        <Route path='/new' component={NewTodo} />
      </Switch>
    </React.Suspense>
  </Router>
)
```

[使用 React Suspense 预缓存图像](https://css-tricks.com/pre-caching-image-with-react-suspense/)
