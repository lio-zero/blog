# 使用 React Hooks 获取数据时避免 Race Condition

[Race Condition](https://zh.wikipedia.org/wiki/%E7%AB%B6%E7%88%AD%E5%8D%B1%E5%AE%B3) （竞态条件）表示当事件的时序影响一段代码的正确性时，就会发生竞态条件。

假设有这样一个场景，我们在点击带有用户姓名的按钮时假装加载他们的个人资料数据。

为了帮助可视化竞态条件，我们将创建一个 `fakeFetch` 函数，该函数实现 0 到 5 秒之间的随机延迟。

```tsx
const fakeFetch = (person) =>
  new Promise((res) =>
    setTimeout(() => res(`${person}'s data`), Math.random() * 5000)
  )
```

## 最初的实现

我们的初始实现将使用按钮来设置当前用户资料。我们使用 `useState` 钩子来实现这一点，并保持以下状态：

- `person` — 用户选择的人
- `data` — 根据选定的人伪获取加载的数据
- `loading` — 当前是否正在加载数据

我们还使用了 `useEffect` 钩子，它在 `person` 更改时执行伪获取。

```tsx
import React, { useState, useEffect } from 'react'

const App = () => {
  const [data, setData] = useState('')
  const [loading, setLoading] = useState(false)
  const [person, setPerson] = useState(null)

  useEffect(() => {
    setLoading(true)
    fakeFetch(person).then((data) => {
      setData(data)
      setLoading(false)
    })
  }, [person])

  return (
    <>
      <button onClick={() => setPerson('O.O')}>O.O 简介</button>
      <button onClick={() => setPerson('D.O')}>D.O 简介</button>
      <button onClick={() => setPerson('K.O')}>K.O 简介</button>
      {person && (
        <>
          <h1>{person}</h1>
          <p>{loading ? 'Loading...' : data}</p>
        </>
      )}
    </>
  )
}
export default App
```

运行上面示例，并单击其中一个按钮，则伪获取将按预期加载数据。

## 命中竞态条件

当我们快速地在用户资料之间切换时，麻烦就来了。考虑到我们的伪获取具有随机延迟，我们很快发现获取结果可能会无序返回。此外，我们选择的个人资料和加载的数据可能不同步。

![数据不同步](https://upload-images.jianshu.io/upload_images/18281896-ea6891aff925ec40.gif?imageMogr2/auto-orient/strip)

这里发生的事情是相对直观的：`useEffect` 钩子中的 `setData(data)` 只有在 `fakeFetch` promise 得到解决后才被调用。无论哪个 promise 最后解决，都将最后调用 `setData`，而不管哪个按钮实际上是最后调用的。

## 取消之前的获取

我们可以通过**取消**任何非最近单击的 `setData` 调用来修复此竞态条件。为此，我们在 `useEffect` 钩子内创建一个布尔变量，并从 `useEffect` 钩子返回一个清理函数，该函数将布尔 `canceld` 变量设置为 `true`。当 promise 解决时，只有当 `canceled` 变量为 `false` 时，才会调用 `setData`。

如果这个描述有点混乱，那么下面的 `useEffect` 钩子的代码示例应该会有所帮助。

```tsx
useEffect(() => {
  let canceled = false

  setLoading(true)
  fakeFetch(person).then((data) => {
    if (!canceled) {
      setData(data)
      setLoading(false)
    }
  })

  return () => (canceled = true)
}, [person])
```

即使上一个按钮单击的 `fakeFetch` promise 稍后解决，其 `canceled` 的变量也将设置为 `true`，并且不会执行 `setData(data)`。

看看示例如何运行：

![数据同步](https://upload-images.jianshu.io/upload_images/18281896-078bcb5543cbaecc.gif?imageMogr2/auto-orient/strip)

完美，无论我们点击了多少次不同的按钮，我们总是只会看到与最后一次按钮点击相关的数据。

[**示例地址**](https://code.juejin.cn/pen/7144624728882282510)
