# parentElement 与 parentNode 的区别

首先，你需要知道元素和节点之间的区别。总之，元素是一种特殊类型的节点，它表示 DOM 树中的单个节点。它不仅可以是元素，还可以是注释、文档、文本节点等。

- `parentElement` 属性返回当前元素的父元素。
- `parentNode` 属性返回当前元素的父节点，即当前元素的直接父元素。

在大多数情况下，`parentElement` 和 `parentNode` 属性返回相同的节点：

```js
// 两者都返回 <html> 元素
document.body.parentNode
document.body.parentElement
```

两者的行为相同。

但是，当当前元素的父节点是文档片段（document fragment）时，两者的行为就不同了。`parentNode` 属性会返回文档片段，而 `parentElement` 属性则返回 `null`。

```js
// 返回文档节点
document.documentElement.parentNode // document

// 返回 null，因为 <html> 元素不存在父元素节点
document.documentElement.parentElement // null
```

也就是说，如果父节点不是元素节点，则 `parentElement` 属性为 `null`。

所以如果你想获取一个元素的父元素，最好使用 `parentElement`。

## 提示

通过检查父元素是否存在，我们可以从给定元素移动到 `html` 标签：

```js
while ((ele = ele.parentElement)) {
  // ...
}
```

下面的代码计算从给定元素到页面顶部的距离：

```js
const distanceToTop = (ele) => {
  let x = 0
  while ((ele = ele.parentElement)) {
    x += ele.offsetTop
  }
  return x
}
```
