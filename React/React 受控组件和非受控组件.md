# React 受控组件和非受控组件

[受控组件](https://reactjs.org/docs/forms.html#controlled-components)（Controlled Component）和[非受控组件](https://reactjs.org/docs/uncontrolled-components.html)（UnControlled Component）取决于表单元素（如 `input`、`textarea`、`select`）是否拥有 `value` 属性。

如果有 `value`，那么它是受控组件。例如：

```jsx
function App() {
  return <input type='text' value='Hello React' />
}
```

这种情况下，任何用户输入的内容都不会被渲染，因为 React 已经显式声明了 `value` 为 **Hello React**，所以不会再被更改。

我们需要使用 `onChange` 事件来更新 `value`：

```jsx
import { useState } from 'react'

function App() {
  const [value, setValue] = useState('Hello React')
  const handleChange = (e) => setValue(e.target.value)
  return <input type='text' value={value} onChange={handleChange} />
}
```

这里我们可以看出，**受控组件不维护自身内部的值，它只会依据 `props` 进行渲染。**

如果没有 `value`，那么它是非受控组件。例如：

```jsx
function App() {
  return <input type='text' />
}
```

该 `input` 初始化会渲染空的值，任何用户输入都会被立即渲染，并且同样可以使用 `onChange` 事件监听其变化。

由此得出，非受控组件会在内部维护自己的 `value`，不在由外界决定。

如果想要为非受控组件一个默认值，可以使用 `defaultValue` 属性，该属性只会在初次渲染起作用。

```jsx
function App() {
  return <input type='text' defaultValue='Hello React' />
}
```

有了默认值的存在，非受控组件在功能上与受控组件几乎一致。

## 为什么选择受控组件？

**React 建议我们始终使用受控组件，这是为什么呢？**

来看一个示例：

```jsx
function App() {
  const handleChange = (e) => {
    console.log(e.target.getAttribute('value'))
    console.log(e.target.value)
  }

  return (
    <input type='text' defaultValue='Hello React' onChange={handleChange} />
  )
}
```

尝试运行它，您会发现，`inputRef.current.value` 会实时更新，但 `inputRef.current.getAttribute('value')` 永远为 `defaultValue` 属性提供的默认值。

而受控组件可以避免这个问题：

```jsx
import { useState } from 'react'

function App() {
  const [value, setValue] = useState('Hello React')
  const handleChange = (e) => {
    setValue(e.target.value)

    console.log(e.target.getAttribute('value'))
    console.log(e.target.value)
  }

  return <input type='text' value={value} onChange={handleChange(e)} />
}
```

当改用受控组件的写法后，`input` 的值由 `props` 决定，`value` 会随着状态的更新而变化，所以 `e.target.getAttribute('value')` 也会随之更新。
