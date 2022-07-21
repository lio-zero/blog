# 使用 React 自动聚焦字段

自动对焦可以让你的应用程序更方便用户使用，有几种方法可以自动聚焦 React 输入框。

要让输入框自动聚焦，最简单的方法是使用 `autoFocus` 属性（注意大小写）：

```html
<input name="username" type="text" autoFocus />
```

由于该属性在各个浏览器的工作方式不一致，React 内部实现了一个 [polyfill](https://github.com/facebook/react/issues/11851#issuecomment-351780994)，会在元素挂载时使用 `focus()` 方法。

但这并总是有用，我们需要自己封装一个。

有两种实现方法：

- 使用 `useCallback()`
- 使用 `useRef()` 和 `useEffect()`

我们会将它编写成一个 Hooks，以便我们可以在项目中重复使用它。

## 使用 `useCallback()`

`useCallback()` 钩子返回一个已记忆的回调函数。它接受两个参数。第一个是要运行的函数，第二个是运行函数所依赖的值数组。

```js
import { useCallback } from 'react'

const useAutoFocus = () => {
  const inputRef = useCallback((inputElement) => {
    if (inputElement) {
      inputElement.focus()
    }
  }, [])

  return inputRef
}

export default useAutoFocus
```

注意到 `useCallback` 的第二个参数是一个空数组，这意味着该函数在组件渲染时只运行一次。

当表单组件渲染时，将设置输入框的引用。它执行 `useCallback()` 钩子中的函数，该函数对输入框调用 `focus()`。

## 使用 `useRef()` 和 `useEffect()`

`useEffect()` 钩子会告诉 React，在组件渲染之后，您需要组件执行一些操作。它接受两个参数。第一个是要运行的函数，第二个是一个依赖项数组，其功能与 `useCallback()` 中的相同。

`useRef()` 钩子对函数组件的作用与 `createRef()` 对基于类组件的作用相同。这个钩子创建了一个普通的 JavaScript 对象，您可以将其传递给一个元素，以保持对它的引用。可以通过对象的 `current` 属性访问此引用。

```js
import { useRef, useEffect } from 'react'

const useAutoFocus = () => {
  const inputRef = useRef(null)

  useEffect(() => {
    if (inputRef.current) {
      inputRef.current.focus()
    }
  }, [])

  return inputRef
}

export default useAutoFocus
```

在上面的代码中，我们创建了对输入框的引用。然后，当组件渲染时，我们使用引用对象的 `current` 属性调用输入框的 `focus()`。

使用示例：

```js
import { useState, useEffect } from 'react'
import ReactDOM from 'react-dom'
import useAutoFocus from './hooks/useAutoFocus'

function App() {
  const emailInput = useAutoFocus()

  return (
    <>
      <form>
        <label>
          用户
          <input name='username' type='text' ref={emailInput} />
        </label>
        <label>
          密码
          <input name='password' type='password' />
        </label>
        <button type='submit'>登录</button>
      </form>
    </>
  )
}

ReactDOM.render(<App />, document.getElementById('root'))
```

## 更多资料

[React Hooks](https://github.com/lio-zero/blog/blob/master/React/React%20Hooks.md)
