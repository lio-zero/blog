# 实现 findIndex 方法

`map` 方法的语法：

```js
arr.findIndex(func)
```

- `arr` 是调用 `map` 的数组。
- `func` 是需要在数组的每个元素上运行的函数。

`findIndex` 方法的基本功能很简单：

它是一个遍历数组的每个元素并应用作为参数传递的函数的方法。`map` 返回满足提供的 `func` 函数的第一个索引，若没有找到对应元素则返回 `-1`。

现在我们了解了需求，因此我们可以继续创建自己的 `findIndex` 方法。以下是我们的新 `findIndex` 方法的代码：

```js
function myFindIndex(func) {
  if (typeof func !== 'function')
    throw new Error('predicate must be a function')

  let index = -1
  for (let i = 0; i < this.length; i++) {
    const newItem = func(this[i], i, this)
    if (newItem) {
      index = i
      break
    }
  }

  return index
}

Object.defineProperty(Array.prototype, 'myFindIndex', {
  value: myFindIndex
})
```

分析：

- `func` 必须是一个函数，否则抛出错误
- 通过 `for` 循环，将**当前值、索引和数组本身**传递给 `func` 函数，这些参数可以通过 `this`（当前调用 `myFindIndex` 方法的数组）获取
- 如果 `func` 返回 `true`，返回当前数组循环的索引，并使用 `break` 跳出循环。否则，返回 `-1`

示例：

```js
const arr = [5, 12, 8, 130, 44]
const isLargeNumber = (element) => element > 13
console.log(arr.myFindIndex(isLargeNumber)) // 3

// 查找数组中首个质数元素的索引
function isPrime(element, index, array) {
  var start = 2
  while (start <= Math.sqrt(element)) {
    if (element % start++ < 1) {
      return false
    }
  }
  return element > 1
}

console.log([4, 6, 8, 12].myFindIndex(isPrime)) // -1
console.log([4, 6, 7, 12].myFindIndex(isPrime)) // 2
```

以下是 MDN 上提供的一个 [`findIndex` Polyfill](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/findIndex#polyfill)：

```js
// https://tc39.github.io/ecma262/#sec-array.prototype.findIndex
if (!Array.prototype.findIndex) {
  Object.defineProperty(Array.prototype, 'findIndex', {
    value: function (predicate) {
      // 1. Let O be ? ToObject(this value).
      if (this == null) {
        throw new TypeError('"this" is null or not defined')
      }

      var o = Object(this)

      // 2. Let len be ? ToLength(? Get(O, "length")).
      var len = o.length >>> 0

      // 3. If IsCallable(predicate) is false, throw a TypeError exception.
      if (typeof predicate !== 'function') {
        throw new TypeError('predicate must be a function')
      }

      // 4. If thisArg was supplied, let T be thisArg; else let T be undefined.
      var thisArg = arguments[1]

      // 5. Let k be 0.
      var k = 0

      // 6. Repeat, while k < len
      while (k < len) {
        // a. Let Pk be ! ToString(k).
        // b. Let kValue be ? Get(O, Pk).
        // c. Let testResult be ToBoolean(? Call(predicate, T, « kValue, k, O »)).
        // d. If testResult is true, return k.
        var kValue = o[k]
        if (predicate.call(thisArg, kValue, k, o)) {
          return k
        }
        // e. Increase k by 1.
        k++
      }

      // 7. Return -1.
      return -1
    }
  })
}
```
