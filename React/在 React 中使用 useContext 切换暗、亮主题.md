# 在 React 中使用 useContext 切换暗/亮主题

在 Web 应用程序中，暗/亮主题切换是很常见的。

本文将展示一种使用 React 的 `useContext` 和 `useState` hooks 实现暗/亮模式切换的方法。

这个示例很简单，我们总共只需要编写两个组件：`ThemeProvider` 和 `App`。

您可以提前在这查看效果：[示例地址](https://code.juejin.cn/pen/7133568917863137318)。

首先，我们先定义 `ThemeProvider` 组件几个必要的类型：

- 我们将 `Theme` 定义为 `light` 或 `dark`，并将 `ThemeContext` 定义为具有两个属性的对象：`theme` 和 `toggleTheme`（它们通过 `useContext` 钩子提供给其他组件）。
- 我们必须确保使用 React 导出 `createContext` 创建 `ThemeContext` 对象。

我们开始编写组件：

我们使用 `useState` 钩子维护 `theme` 状态，并创建一个 `toggleTheme` 函数，该函数将在 `light` 和 `dark` 之间切换状态。

为了简单起见，我们简单地根据 `theme` 状态当前是 `light` 还是 `dark` 来设置文档正文的 `color` 和 `backgroundColor` 样式。最后，我们返回 `ThemeContext.Provider`，其值设置为具有 `theme` 和 `toggleTheme` 属性的对象，并在 `ThemeContext.Provider` 组件中渲染子级。

```ts
import React, { createContext, useState } from 'react'

type Theme = 'light' | 'dark'
type ThemeContext = { theme: Theme; toggleTheme: () => void }

const ThemeContext = createContext<ThemeContext>({} as ThemeContext)

const ThemeProvider: React.FC = ({ children }) => {
  const [theme, setTheme] = useState<Theme>('light')
  const toggleTheme = () => {
    setTheme(theme === 'light' ? 'dark' : 'light')
  }

  const color = theme === 'light' ? '#333' : '#FFF'
  const backgroundColor = theme === 'light' ? '#FFF' : '#333'

  document.body.style.color = color
  document.body.style.backgroundColor = backgroundColor

  return <ThemeContext.Provider value={{ theme, toggleTheme }}>{children}</ThemeContext.Provider>
}
```

在 `App` 组件中，我们使用 `useContext` 钩子来访问 `theme` 字符串和 `toggleTheme` 函数。我们创建了一个可以切换主题的按钮，仅用于 `theme` 来确定向用户显示的内容：**Switch to dark mode** 或 **Switch to light mode**。

```ts
import React, { useContext } from 'react'

const App: React.FC = () => {
  const { theme, toggleTheme } = useContext(ThemeContext)

  return (
    <>
      <div>Hi friend!</div>
      <button onClick={toggleTheme}>Switch to {theme === 'light' ? 'dark' : 'light'} mode</button>
    </>
  )
}
```

接着，我们只需将整个 `App` 包装在 `ThemeProvider` 组件中。当然，我们不需要在实际项目应用层面这样做，我们只需要确保任何需要 `theme` 或 `toggleTheme` 的组件都在我们的 Provider 的子树中即可。

```ts
import ReactDOM from 'react-dom'

ReactDOM.render(
  <ThemeProvider>
    <App />
  </ThemeProvider>,
  document.getElementById('app')
)
```
