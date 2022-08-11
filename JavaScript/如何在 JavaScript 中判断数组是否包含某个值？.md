# 如何在 JavaScript 中判断数组是否包含某个值？

- 常规循环：递减循环效率更高通常最高。

```js
const arr = ['red', 'yellow', 'black', 'white', 'yellow']

function contains(a, obj) {
  let i = a.length
  while (i--) {
    if (a[i] === obj) {
      return true
    }
  }
  return false
}

contains(arr, 'yellow') // true
contains(arr, 'plum') // false
```

- [`Array.prototype.indexOf()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/indexOf) 判断数组中是否存在某个值，如果存在，则返回数组元素的下标，否则返回 `-1`。

```js
arr.indexOf('plum') // -1
arr.indexOf('yellow') // 1
arr.indexOf('yellow', 2) // 4

arr.indexOf('red') != -1 // true
```

- [`Array.prototype.includes()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/includes) 判断数组中是否存在某个值，如果存在返回 `true`，否则返回 `false`。

```js
arr.includes('red') // true
arr.includes('plum') // false
```

- [`Array.prototype.find()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/find) 返回数组中满足条件的**第一个元素的值**，如果没有，返回 `undefined`。

```js
arr.find((item) => item === 'black' && item) // "black"
```

- [`Array.prototype.findIndex()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/findIndex) 返回数组中满足条件的第一个元素的下标，如果没有找到，返回 `-1`。

```js
arr.findIndex((item) => item === 'white') // 3
```

许多框架都有类似的方法：

- jQuery：[`$.inArray()`](https://api.jquery.com/jquery.inarray/)
- Underscore.js：[`_.contains()`](https://underscorejs.org/#contains)
- Lodash：[`_.includes()`](https://lodash.com/docs/4.17.15#includes)
- Ramda：[`R.includes()`](https://ramdajs.com/docs/#includes)

## 更多资料

Stack Overflow 有很多这方面的讨论，包括检查数组对象，以上提供方法的基准测试等，详细内容请查阅 [How do I check if an array includes a value in JavaScript?](https://stackoverflow.com/questions/237104/how-do-i-check-if-an-array-includes-a-value-in-javascript)。
