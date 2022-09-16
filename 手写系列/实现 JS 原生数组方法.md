# 实现 JS 原生数组方法

理解数组方法的内部实现，有助于帮助我们更好的使用它们。这些数组方法都可以使用 [core-js](https://github.com/zloirock/core-js#ecmascript-array) 标准库提供的 polyfill 解决。

如果您还不熟悉数组方法的话，可以阅读我写的另一篇 [JavaScript 数组方法总结](https://github.com/lio-zero/blog/blob/main/JavaScript/JavaScript%20%E6%95%B0%E7%BB%84%E6%96%B9%E6%B3%95%E6%80%BB%E7%BB%93.md)。

以下只实现了一部分的数组方法，用最简单的方式实现它们。

我们用一个数组的测试下面的方法：

```js
const arr = [1, 2, 3, 4, 5]
```

您可以选择以下方式将它们添加到原型：

```js
Array.prototype.methods = function (callback) {}

// 使用 Object.defineProperty 在数组构造函数的原型 Array.prototype 内创建一个新属性
Object.defineProperty(Array.prototype, 'methods', {
  value: methods
})
```

> **注意**：不要在您的生产代码中遵循以下方法。这可能会损坏现有代码。

## push

```js
Array.prototype.myPush = function (...args) {
  const length = args.length
  let len = this.length
  for (var i = 0; i < length; i++) {
    this[len] = args[i]
    len++
  }
  return len
}

arr.myPush(1)
arr.myPush(1, 2)
```

## join

```js
Array.prototype.myJoin = function (split = ',') {
  let str = ''
  for (let i = 0; i < this.length; i++) {
    str = i === 0 ? `${str}${this[i]}` : `${str}${split}${this[i]}`
  }
  return str
}

arr.myJoin() // '1,2,3,4,5'
arr.myJoin('-') // '1-2-3-4-5'
```

## fill

```js
Array.prototype.myFill = function (value, start = 0, end) {
  end = end || this.length
  for (let i = start; i < end; i++) {
    this[i] = value
  }
  return this
}

arr.myFill('O.O', 0, 1) // ['O.O', 'O.O', 3, 4, 5]
```

## includes

`includes` 可以查找到 `NaN`。

```js
Array.prototype.myIncludes = function (searchElement, fromIndex = 0) {
  if (fromIndex < 0) fromIndex = this.length + fromIndex
  for (let i = fromIndex; i < this.length; i++) {
    if (Object.is(this[i], searchElement)) return true
  }
  return false
}

arr.myIncludes(2) // true
arr.myIncludes(1, 1) // false
;[1, 2, 3, 4, 5, NaN].myIncludes(NaN) // true
```

## find

`find` 返回值。

```js
Array.prototype.myFindIndex = function (callback) {
  for (let i = 0; i < this.length; i++) {
    if (callback(this[i], i, this)) return this[i]
  }

  return undefined
}

arr.myFindIndex((item) => item === 2) // 2
arr.myFindIndex((item) => item === 6) // undefined
```

## findIndex

`findIndex` 返回索引。

```js
Array.prototype.myFindIndex = function (callback) {
  for (let i = 0; i < this.length; i++) {
    if (callback(this[i], i, this)) return i
  }

  return -1
}

arr.myFindIndex((item) => item === 2) // 3
arr.myFindIndex((item) => item === 6) // -1
```

MDN 更为完整的 [`findIndex` Polyfill](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/findIndex#polyfill)。

## flat

这里直接全部展平，没有做根据参数进行判断展平层数。

```js
Array.prototype.myFlat = function () {
  let arr = this
  while (arr.some((item) => Array.isArray(item))) {
    arr = [].concat(...arr)
  }
  return arr
}

;[1, [2, [4, 5]], [6, 7]].myFlat() // [1, 2, 3, 4, 5, 8, 9]
```

## map

```js
Array.prototype.myMap = function (callback) {
  const ret = []
  for (let i = 0; i < this.length; i++) {
    ret.push(callback(this[i], i, this))
  }

  return ret
}

arr.myMap((item) => item + 1) // [2, 3, 4, 5, 6]
```

## some

```js
Array.prototype.mySome = function (func) {
  let ret = false
  for (let i = 0; i < this.length; i++) {
    if (func(this[i], i, this)) {
      ret = true
      break
    }
  }

  return ret
}

arr.mySome((item) => item >= 2) // true
arr.mySome((item) => item >= 6) // false
```

## every

```js
Array.prototype.myEvery = function (callback) {
  let ret = true
  for (let i = 0; i < this.length; i++) {
    if (!callback(this[i], i, this)) {
      ret = false
      break
    }
  }

  return ret
}

arr.mySome((item) => item >= 2) // true
arr.mySome((item) => item >= 6) // false
```

## forEach

```js
Array.prototype.myForEach = function (callback) {
  for (var i = 0; i < this.length; i++) {
    callback(this[i], i, this)
  }
}

arr.myForEach(console.log)
// 1 0 [1, 2, 3, 4, 5]
// 2 1 [1, 2, 3, 4, 5]
// 3 2 [1, 2, 3, 4, 5]
// 4 3 [1, 2, 3, 4, 5]
// 5 4 [1, 2, 3, 4, 5]
```

## filter

```js
Array.prototype.myFilter = function (callback) {
  const ret = []
  for (let i = 0; i < this.length; i++) {
    if (callback(this[i], i, this)) {
      ret.push(this[i])
    }
  }

  return ret
}

arr.myFilter((item) => item % 2 === 0) // [2, 4]
```

## reduce

考虑是否有初始值。

```js
Array.prototype.myReduce = function (callback, initialValue) {
  let total = initialValue || this[0]
  let start = initialValue ? 0 : 1
  for (let i = start; i < this.length; i++) {
    total = callback(total, this[i], i, this)
  }
  return total
}

arr.myReduce((accumulator, elem) => (accumulator += elem)) // 15
arr.myReduce((accumulator, elem) => (accumulator += elem), 100) // 115
```

## splice

`splice` 实现是最复杂的，您可以阅读 [字节：模拟实现 Array.prototype.splice](https://github.com/sisterAn/JavaScript-Algorithms/issues/138) 了解更加细致的实现过程。

其他数组方法实现较为简单，基本一眼就能看懂，也就没有进行过多解释。
