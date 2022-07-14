# 实现 map 方法

`map` 方法的语法：

```js
arr.map(func)
```

- `arr` 是调用 `map` 的数组。
- `func` 是需要在数组的每个元素上运行的函数。

`map` 方法的基本功能很简单：

它是一个遍历数组的每个元素并应用作为参数传递的函数的方法。`map` 的返回类型同样是一个数组，`func` 应用于每个元素。

现在我们了解了需求，因此我们可以继续创建自己的 `map` 方法。以下是我们的新 `map` 方法的代码：

```js
function myMap(func) {
  let destArr = []
  for (let i = 0; i < this.length; i++) {
    destArr.push(func(this[i], i, this))
  }

  return destArr
}
```

分析：

此方法接受名为 `func` 的参数。它只不过是一个需要在数组的每个元素上调用的函数。

代码的其他部分很容易解释。我们将重点关注这一行：`destArr.push(func(this[i], i, this))`。

这一行做了两件事：

- 将更改推入到 `destArr`
- 将**当前值、索引和数组本身**传递给 `func` 函数，这些参数可以通过 `this`（调用 `myMap` 方法的数组）获取

现在让我们看看如何执行 `myMap` 函数。不建议使用以下方法向现有的原始数据类型添加新方法，但为了本文的目的，我们仍将这样做。

> **注意**：不要在您的生产代码中遵循以下方法。这可能会损坏现有代码。

```js
Object.defineProperty(Array.prototype, 'myMap', {
  value: myMap
})
```

我们使用 `Object.defineProperty` 在数组构造函数的原型 `Array.prototype` 内创建一个新属性。

一旦完成，我们就可以在数组上执行新的 `map` 方法了。

```js
const arr = [1, 2, 3]
const newArr = arr.myMap((item) => item + 1)
console.log(newArr) // [2, 3, 4]
```
