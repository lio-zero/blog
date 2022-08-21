# `<>` 和 `React.Fragment` 的区别

`<>` 是 `React.Fragment` 的简写标签。它允许我们对元素列表进行分组，而无需将它们包装到新节点中。

> **建议**：基本上，我们应该在任何时候使用 `React.Fragment` 或 `<>`，它可以避免不必要的 `div` 包装器，得到一个更加清晰的标签结构。

所以我们可以这样做：

```js
return (
  <>
    <Header />
    <Navigation />
    <Main />
    <Footer />
  </>
)
```

它们之间唯一的区别是简写版本不支持 `key` 属性。

以下是在多行字符串中插入新行（`br`）标签的常见示例：

```js
{
  str.split('\\n').map((item, index) => {
    return (
      <Fragment key={index}>
        {item}
        <br />
      </Fragment>
    )
  })
}
```
