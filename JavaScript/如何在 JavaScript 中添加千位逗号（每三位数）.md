# 如何在 JavaScript 中添加千位逗号（每三位数）

今天讲一个常见的场景：在你输入金额时，每三位数自动添加一个逗号。你可能需要对一段长数字进行一些基本的数字格式化。

转换如下：

- 100 —> 100
- 1000 —> 1,000
- 1000000 —> 1,000,000

有三种相当简单的方法可以在 JavaScript 中执行此操作。

## toLocaleString()

[`toLocaleString()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/toLocaleString) 方法返回这个数字在特定语言环境下的表示字符串。

```js
const addCommas = (num) => num.toLocaleString()

addCommas(100) // "100"
addCommas(1000) // "1,000"
addCommas(1000000) // "1,000,000"
```

## Intl.NumberFormat

> [`Intl.NumberFormat`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Intl/NumberFormat) 是对语言敏感的格式化数字类的构造器类。

```js
const number = 123456.789

console.log(new Intl.NumberFormat('de-DE').format(number)) // 123.456,789
console.log(new Intl.NumberFormat('ja-JP').format(number)) // 123,456.789
console.log(
  new Intl.NumberFormat('en-IN', { maximumSignificantDigits: 3 }).format(number)
) // 1,23,000
```

## 正则表达式

另一种选择是使用正则表达式。如果我们的输入是数字，我们将需要确保在应用正则表达式之前将其隐式转换为字符串。

```js
const addCommas = (num) =>
  num.toString().replace(/(\d)(?=(\d{3})+(?!\d))/g, '$1,')

addCommas(100) // "100"
addCommas(1000) // "1,000"
addCommas(1000000) // "1,000,000"
```
