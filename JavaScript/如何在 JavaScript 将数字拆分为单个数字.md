# 如何在 JavaScript 将数字拆分为单个数字

## String.prototype.split()

[`String.prototype.split()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/split) 方法用于把一个字符串分割成字符串数组。

- 将数字转换为字符串，将其拆分为字符串数组（拼接或其他方法）。
- 使用 `String.prototype.split()` 与 `Array.prototype.map()` 方法把每个字符串转换成数字。

```js
const splitToDigit = (n) => (n + '').split('').map(Number)

splitToDigit(100) // [1, 0, 0]
```

## 扩展运算符

使用扩展运算符（`...`） 和 `Array.prototype.map()` 来简化上面的操作。

```js
const splitToDigit = (n) => [...(n + '')].map(Number)

splitToDigit(100)
```

**注意**：扩展运算是通过循环来张开可迭代元素，所以需要先转为字符串形式

## 运算

以数学操作的形式，将数字拆分为单个数字。

```js
const splitToDigit = (n) => {
  let num = []
  while (n > 0) {
    num.push(n % 10)
    n = parseInt(n / 10)
  }
  return num.reverse()
}

splitToDigit(100) // [1, 0, 0]
```
