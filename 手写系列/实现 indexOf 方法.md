# 实现 indexOf 方法

JS 为字符串和数组提供了原生 `indexOf` 方法：

`String.prototype.indexOf(searchElement [, fromIndex])` 方法用于返回在字符串中第一次出现的指定的子字符串的索引，或者在没有找到子字符串时返回 -1。

这个方法接受两个参数：

- `searchValue` — 要在字符串中查找的子字符串。
- `fromIndex`（可选）— 指定开始搜索的索引位置。如果未指定，则默认为 0。如果传递了负数，则会将其转换为字符串长度加上该值。

> **注意**：`indexOf()` 方法是区分大小写的。

`Array.prototype.indexOf(searchElement [, fromIndex])` 方法用于返回数组中第一个找到的指定元素的索引，如果没有找到该元素，则返回 -1。

这个方法接受两个参数：

- `searchElement` — 要在数组中查找的元素。
- `fromIndex`（可选）— 指定开始搜索的索引位置。如果未指定，则默认为 0。如果传递了负数，则会将其转换为数组的长度加上该值，以确保开始搜索的索引位置始终在数组的范围内。

`Array.prototype.indexOf()` 方法实现如下：

```js
function arrayIndexOf(arr, searchElement, fromIndex = 0) {
  if (fromIndex < 0) fromIndex += arr.length
  for (let i = fromIndex; i < arr.length; i++) {
    if (arr[i] === searchElement) return i
  }
  return -1
}

arrayIndexOf([1, 2, 3, 4], 3) // 2
arrayIndexOf([1, 2, 3, 4], 5) // -1
arrayIndexOf([1, 2, 3, 4], 2, 2) // -1
```

`String.prototype.indexOf()` 方法实现如下：

```js
function stringIndexOf(str, searchValue, fromIndex = 0) {
  if (fromIndex < 0) fromIndex += str.length
  for (let i = fromIndex; i < str.length; i++) {
    if (str.substring(i, i + searchValue.length) === searchValue) return i
  }
  return -1
}

stringIndexOf('hello world', '') // 0
stringIndexOf('hello world', '', 0) // 0
stringIndexOf('hello world', '', 3) // 3
stringIndexOf('hello world', '', 8) // 8
stringIndexOf('hello world', 'o') // 4
stringIndexOf('hello world', 'o', 5) // 7
```

需要注意，如果传递了负数作为 `fromIndex` 参数，则这两个函数会将其转换为数组或字符串的长度加上该值，以确保开始搜索的索引位置始终在数组或字符串的范围内。

它们各自还有一个 `lastIndexOf` 方法，详细内容可以自行查阅了解。
