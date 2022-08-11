# 如何在 JavaScript 中将数组拆分为一组数组

有时我们有一个数组，我们可能想将其拆分为多个数组：

```js
const nums = [1, 2, 3, 4, 5, 6, 7, 8, 9]
```

假如我们需要把它拆分为三个子数组的数组，如下所示：

```js
const result = [
  [1, 2, 3],
  [4, 5, 6],
  [7, 8, 9]
]
```

**我们该如何实现？**

一种快速的解决方案如下。它将返回一个新数组，并且不会改变原数组 `nums`。

```js
const createGroups = (arr, numGroups) => {
  const perGroup = Math.ceil(arr.length / numGroups)
  return new Array(numGroups)
    .fill('')
    .map((_, i) => arr.slice(i * perGroup, (i + 1) * perGroup))
}

createGroups(nums, 3) // [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
createGroups(nums, 4) // [[1, 2, 3], [4, 5, 6], [7, 8, 9], []]
```

你可以指定不同的分组长度，来查看效果。
