# parentElement 与 parentNode 的区别

首先，您需要知道元素和节点之间的区别。总之，元素是一种特殊类型的节点，它表示 DOM 树中的单个节点。它不仅可以是元素，还可以是注释、文档、文本节点等。

在大多数情况下，`parentElement` 和 `parentNode` 属性返回相同的节点：

```js
// 两者都返回 <html> 元素
document.body.parentNode
document.body.parentElement
```

唯一的区别是，如果父节点不是元素节点，则 `parentElement` 属性可以为 `null`：

```js
// 例外
// 返回文档节点
document.documentElement.parentNode

// 返回 null，因为 <html> 元素不存在
// 有一个父元素节点
document.documentElement.parentElement
```

## 提示

通过检查父元素是否存在，我们可以从给定元素移动到 `html` 标签：

```js
while ((ele = ele.parentElement)) {
  // ...
}
```

下面的代码段计算从给定元素到页面顶部的距离：

```js
const distanceToTop = (ele) => {
  let x = 0
  while ((ele = ele.parentElement)) {
    x += ele.offsetTop
  }
  return x
}
```
