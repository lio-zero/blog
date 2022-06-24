# 模拟 setInterval

**目的**：避免 `setInterval` 因执行时间导致的间隔执行时间不一致。

## arguments.callee

```js
setTimeout(function () {
  console.log(new Date())
  setTimeout(arguments.callee, 1000)
}, 0)
```

这里需要注意两点：

- 这里不能使用箭头函数，因为箭头函数没有 `argument` 对象。
- `argument` 已经从 Web 标准中删除，并且在严格模式（`'use strict'`）下，不能使用 [`arguments.callee`](https://developer.mozilla.org/es/docs/Web/JavaScript/Reference/Global_Objects/Function/arguments)。

## 递归 + `setTimeout()`

```js
function infinite() {
  console.log(new Date())
  return setTimeout(infinite, 1000)
}

infinite()
```
