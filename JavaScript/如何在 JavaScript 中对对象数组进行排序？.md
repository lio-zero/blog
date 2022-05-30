# 如何在 JavaScript 中对对象数组进行排序？

在开始之前，我们先来了解一下 `Array.prototype.sort()` 的一些需要注意的地方。

## Array.prototype.sort()

> MDN：[`Array.prototype.sort()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/sort) 方法用[原地算法](https://en.wikipedia.org/wiki/In-place_algorithm)对数组的元素进行排序，并返回数组。

**在使用 `Array.prototype.sort()` 方法时，我们需要注意两点**：

- 该方法默认会把数组中的所有元素先转换为 String 再排序。

- 字符串根据 [ASCII](https://www.asciim.cn/) 码进行排序。

```js
const months = ['March', 'Jan', 'Feb', 'Dec']
months.sort()
console.log(months) // ["Dec", "Feb", "Jan", "March"]

const arr = [1, 30, 4, 21, 100000]
array1.sort()
console.log(arr) // [1, 100000, 21, 30, 4]
```

可以看到，原始值数组在排序时，会根据上面两点提到的规则进行排序。我们该如何解决？往下看。

`Array.prototype.sort()` 是一个高阶函数，他可以传入一个指定按某种顺序进行排列的函数。

```js
const arr = [8, 2, 1, 4, 5, 0]

arr.sort((a, b) => {
  if (a > b) return 1
  if (b > a) return -1
  return 0
}) // [0, 1, 2, 4, 5, 8]
```

他更简洁的方式是：

```js
const arr = [8, 2, 1, 4, 5, 0]
// 按升序排序
arr.sort((a, b) => a - b) // [0, 1, 2, 4, 5, 8]
// 按降序排序
arr.sort((a, b) => b - a) // [8, 5, 4, 2, 1, 0]
```

该技巧取决于 `Array.prototype.sort()` 期望一个正值或负值来执行两个元素之间的交换。

如果您使用的是字符串数组，可以使用 [`String.prototype.localeCompare()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/localeCompare)，因为它提供了更大的灵活性，通过考虑特定的区域设置及其独特的需求：

```js
const arr = ['Hi', 'Hola', 'Hello']

// 按升序排序
arr.sort((a, b) => a.localeCompare(b)) // ['Hello', 'Hi', 'Hola']
// 按降序排序
arr.sort((a, b) => b.localeCompare(a)) // ['Hola', 'Hi', 'Hello']
```

## 对象可以按照某个属性排序

根据不同的场景，您可能需要按键、值或日期字段对 JavaScript 数组对象进行排序。

使用 `Array.prototype.sort()` 和 `String.prototype.localeCompare()` 对字符串数据进行排序。

```js
let arr = [
  { name: 'HTML', dataTime: '1999-01-20' },
  { name: 'JavaScript', dataTime: '2000-07-22' },
  { name: 'CSS', dataTime: '2020-08-20' }
]

const sortData = (data, order = 'asc') => {
  if (order == 'asc') {
    return data.sort((a, b) => a.dataTime.localeCompare(b.dataTime))
  } else {
    return data.sort((a, b) => b.dataTime.localeCompare(a.dataTime))
  }
  return data
}

console.log(sortData(arr))
```
