# 修复 React 中的 “Cannot read property 'map' of undefined” 错误

这可能是你在开始使用 React 时会遇到的最常见错误之一：

```text
Cannot read property 'map' of undefined
```

在这篇文章中，我们将学习如何修复它。

## 为什么会发生这种情况？

**原因**：您尝试 `map` 的变量是 `undefined`。它最终可能是一个数组，但由于 React 的异步特性，当变量为 `undefined` 时，您至少会遇到一次渲染。

以下提供一个示例，其中，我们从 API 获取一些数据，并使用这些数据设置状态。

```jsx
function MyComponent() {
  const [data, setData] = useState()

  useEffect(() => {
    fetch('/api/data')
      .then((res) => res.json())
      .then((data) => {
        setData(data)
      })
      .catch((err) => {
        console.log(err)
      })
  }, [])

  return (
    <ul>
      {data.map((item) => (
        <li key={item.id}>{item.name}</li>
      ))}
    </ul>
  )
}
```

这段代码可能看起来不错，但我们的数据获取需要一些时间，React 不会等到数据获取（或任何异步操作）发生后才第一次渲染 JSX。这意味着，在获取数据时，React 正在尝试运行 `data.map(...)`。

因为我们在 `useState` 钩子中没有为 `data` 提供初始值，所以 `data` 为 `undefined`。正如我们从错误消息中所知道的，尝试调用 `undefined` 的 `map` 是有问题的！

## 修复选项一：将变量默认为空数组

第一个快速修复方法对于您的用例来说可能足够了：只需在等待获取数据时将有状态变量默认为数组。例如：

```jsx
function MyComponent() {
  // useState 中的空数组
  const [data, setData] = useState([])

  useEffect(() => {
    fetch('/api/data')
      .then((res) => res.json())
      .then((data) => {
        setData(data)
      })
      .catch((err) => {
        console.log(err)
      })
  }, [])

  return (
    <ul>
      {data.map((item) => (
        <li key={item.id}>{item.name}</li>
      ))}
    </ul>
  )
}
```

其工作的原因是，当数据获取发生时，React 将调用空 `data` 数组上的 `map` 方法。这很好，不会渲染任何内容，也不会出现错误。一旦从 API 调用加载数据，我们的 `data` 状态将被设置，列表将正确渲染。

## 修复选项二：显示加载指示器

上一个修复很简单，但在加载数据之前不显示任何内容可能不是最佳的用户体验。相反，我们可以选择显示加载指示器。有几种方法可以做到这一点，但其中一种更简单的方法是添加一个称为 `loading` 的有状态变量。

```jsx
function MyComponent() {
  const [data, setData] = useState([])
  const [loading, setLoading] = useState(false)

  useEffect(() => {
    setLoading(true)
    fetch('/api/data')
      .then((res) => res.json())
      .then((data) => {
        setData(data)
      })
      .catch((err) => {
        console.log(err)
      })
      .finally(() => {
        setLoading(false)
      })
  }, [])

  if (loading) {
    return <p>正在加载数据...</p>
  }

  return (
    <ul>
      {data.map((item) => (
        <li key={item.id}>{item.name}</li>
      ))}
    </ul>
  )
}
```

这是非常简单和有效的！当数据开始获取时，我们将 `loading` 设置为 `true`。获取完成后，我们将 `loading` 设置为 `false`。注意，我们在 Promise 中使用 `finally` 方法，因为无论获取数据成功还是失败，它都将运行。

## 说到获取失败

我们可能应该处理获取失败的情况。此外，如果 `data` 变量不是数组，我们可以向用户显示错误消息。后一点对于确保我们永远不会尝试访问非数组上的 `map` 属性非常重要，因为它根本不起作用。

```jsx
function MyComponent() {
  const [data, setData] = useState([])
  const [loading, setLoading] = useState(false)
  const [error, setError] = useState()

  useEffect(() => {
    setLoading(true)
    fetch('/api/data')
      .then((res) => res.json())
      .then((data) => {
        setData(data)
      })
      .catch((err) => {
        setError(err)
      })
      .finally(() => {
        setLoading(false)
      })
  }, [])

  if (loading) {
    return <p>正在加载数据...</p>
  }

  if (error || !Array.isArray(data)) {
    return <p>加载数据时出错!</p>
  }

  return (
    <ul>
      {data.map((item) => (
        <li key={item.id}>{item.name}</li>
      ))}
    </ul>
  )
}
```

现在，我们有了一种非常安全的方法来处理异步操作，而不会出现可怕的 “cannot read property ‘map’ of undefined” 错误！
