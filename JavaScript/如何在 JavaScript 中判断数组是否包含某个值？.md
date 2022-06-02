# 如何在 JavaScript 中判断数组是否包含某个值？

- [`Array.prototype.indexOf()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/indexOf) 判断数组中是否存在某个值，如果存在，则返回数组元素的下标，否则返回 `-1`。

```js
const arr = ['red', 'yellow', 'black', 'white', 'yellow']

arr.indexOf('plum') // -1
arr.indexOf('yellow') // 1
arr.indexOf('yellow', 2) // 4

if (arr.indexOf('red') != -1) {
  console.log('存在')
} // 存在
```

- [`Array.prototype.includes()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/includes) 判断数组中是否存在某个值，如果存在返回 `true`，否则返回 `false`。

```javascript
arr.includes('red') // true
arr.includes('plum') // false

if (arr.includes('red')) {
  console.log('存在')
} // 存在
```

- [`Array.prototype.find()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/find) 返回数组中满足条件的**第一个元素的值**，如果没有，返回 `undefined`。

```js
arr.find((item) => item === 'black' && item) // "black"
```

- [`Array.prototype.findIndex()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/findIndex) 返回数组中满足条件的第一个元素的下标，如果没有找到，返回 `-1`。

```js
arr.findIndex((item) => item === 'white') // 3
```
