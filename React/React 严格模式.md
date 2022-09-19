# React 严格模式

你可以使用 `React.StrictMode` 内置组件，用于启用一组检查，以执行 React 并向您发出警告。

一种简单的方法是将整个根组件（这里是 App）包装在 `<React.StrictMode></React.StrictMode>` 中：

```js
// main.js
import React from 'react'
import ReactDOM from 'react-dom'

ReactDOM.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>,
  document.getElementById('root')
)
```

您也可以通过包装应用程序的一个或多个组件来使用它：

```js
import React from 'react'

function Hello() {
  return (
    <>
      <React.StrictMode>{/* ... */}</React.StrictMode>
    </>
  )
}
```

该组件的主要用例之一是用作自动化的最佳实践、潜在问题和弃用检查。

它无法捕捉所有内容，但您在这里有很多不错的检查可以帮助您解决开发问题。

它在 React 16.3 中引入，它不会渲染成任何可见 UI，对生产环境没有影响，因此您可以始终将组件保留在代码库中。在开发中使用，它将在浏览器控制台中打印有用的警告。

至于它会给出哪些警告、检查哪些内容，可以查阅官网的 [Strict Mode](https://reactjs.org/docs/strict-mode.html) 章节，这些内容随时在发生变化，这里就过多介绍。
