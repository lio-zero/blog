# 如何在 JavaScript 中对对象数组进行排序？

在开始之前，我们先来了解一下 `Array.prototype.sort()` 的一些需要注意的地方。

## Array.prototype.sort()

> MDN：[`Array.prototype.sort()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/sort) 方法用[原地算法](https://en.wikipedia.org/wiki/In-place_algorithm) 对数组的元素进行排序，并返回数组。默认排序顺序是在将元素转换为字符串，然后比较它们的 UTF-16 代码单元值序列时构建的。

默认情况下，`sort` 方法按字典序对字符串进行排序。

```js
const months = ['March', 'Jan', 'Feb', 'Dec']
months.sort()
console.log(months) // ["Dec", "Feb", "Jan", "March"]

const arr = [1, 30, 4, 21, 100000]
arr.sort()
console.log(arr) // [1, 100000, 21, 30, 4]
```

可以看到，数组在排序时，会根据上述提到的规则进行排序。

**我们该如何正确排序？**

`Array.prototype.sort()` 是一个高阶函数，它可以传入一个指定按某种顺序进行排列的函数。

```js
const arr = [8, 2, 1, 4, 5, 0]

arr.sort((a, b) => {
  if (a > b) return 1
  if (b > a) return -1
  return 0
}) // [0, 1, 2, 4, 5, 8]
```

它更简洁的方式是：

```js
const arr = [8, 2, 1, 4, 5, 0]
// 按升序排序
arr.sort((a, b) => a - b) // [0, 1, 2, 4, 5, 8]
// 按降序排序
arr.sort((a, b) => b - a) // [8, 5, 4, 2, 1, 0]
```

该技巧取决于 `Array.prototype.sort()` 期望一个正值或负值来执行两个元素之间的交换。

如果你使用的是字符串数组，可以使用 [`String.prototype.localeCompare()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/localeCompare)，因为它提供了更大的灵活性，通过考虑特定的区域设置及其独特的需求：

```js
const arr = ['Hi', 'Hola', 'Hello']

// 按升序排序
arr.sort((a, b) => a.localeCompare(b)) // ['Hello', 'Hi', 'Hola']
// 按降序排序
arr.sort((a, b) => b.localeCompare(a)) // ['Hola', 'Hi', 'Hello']
```

还有几点需要注意：

- `sort` 方法不会保留原始数组的顺序。如果你希望保留原始数组的顺序，则可以使用 `Array.prototype.slice` 等方法克隆数组，然后对克隆的数组进行排序。
- `sort` 方法在不同浏览器中的实现可能略有不同。
- 在使用 `sort` 方法时，应注意数组的类型。
- 如果你希望对数组的子元素进行排序，则需要确保数组中的所有元素都具有该子元素。

最重要的一点还是编写 `sort` 方法比较函数，只要这一步做好了，其他注意点都可以得以解决。

## 对象可以按照某个属性排序

根据不同的场景，你可能需要按价格、日期等字段对 JavaScript 数组对象进行排序。

我们可以使用 `Array.prototype.sort()` 和 `String.prototype.localeCompare()` 对字符串数据进行排序。

```js
let arr = [
  { name: 'HTML', dataTime: '1999-01-20' },
  { name: 'JavaScript', dataTime: '2000-07-22' },
  { name: 'CSS', dataTime: '2020-08-20' }
]

const sortData = (data, order = 'asc') => {
  if (order === 'asc') {
    return data.sort((a, b) => a.dataTime.localeCompare(b.dataTime))
  } else {
    return data.sort((a, b) => b.dataTime.localeCompare(a.dataTime))
  }
  return data
}

console.log(JSON.stringify(sortData(arr), null, 2))
/*
[
  {
    "name": "HTML",
    "dataTime": "1999-01-20"
  },
  {
    "name": "JavaScript",
    "dataTime": "2000-07-22"
  },
  {
    "name": "CSS",
    "dataTime": "2020-08-20"
  }
]
*/
```
