# 如何在 JavaScript 中判断一个值是否为数组？

- 使用数组自带 `Array.prototype.isArray()` 方法来检查值是否为数组。

```js
const str = 'abc'
const arr = [1, 2, 3]

console.log(arr.isArray(arr)) // true
```

- 如果环境不支持 `Array.prototype.isArray()` 方法，则可以使用 `polyfill` 实现，使用 `Object.prototype.toString.call(arr)` 进行类型判断。

```js
// 旧浏览器
function isArray(value) {
  return Object.prototype.toString.call(value) === '[object Array]'
}

// 旧浏览器兼容
if (!Array.isArray) {
  Array.isArray = function (value) {
    return Object.prototype.toString.call(value) === '[object Array]'
  }
}

isArray(arr) // true
isArray(str) // false
```

- 使用 `instanceof` 检验构造函数的 `prototype` 属性是否出现在对象的原型链中，返回一个布尔值。

```js
arr instanceof Array // true
```

- 使用 `constructor` 判断该变量的构造函数是否为 Array

```js
arr.constructor === Array // true
```

- 使用一些提供好的函数库，如 Lodash 的 `_.isArray` 方法检查 `value` 是否归类为 Array 对象。

```js
console.log(_.isArray(arr)) // true
console.log(_.isArray(str)) // false
```

我们一般会组合前两种方法实现一个完美的 `isArray`：

```js
const isArray =
  Array.isArray || ((list) => ({}.toString.call(list) === '[object Array]'))
```

npm 上就有一个 [isarray](https://www.npmjs.com/package/isarray) 包使用这种方式。

**注意**：

- `typeof` 检测出的数组为 `object`。
- 使用 `instanceof` 和 `constructor` 检测是否为数组在一定程度上是便利、好用的，但是也存在着一些问题。主要是在判断上，可能会存在多个全局环境的问题（`iframe`）这种情况不多见，详细可以网上查查。
