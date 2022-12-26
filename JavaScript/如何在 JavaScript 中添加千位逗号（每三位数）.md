# 如何在 JavaScript 中添加千位逗号（每三位数）

今天讲一个常见的场景：在你输入金额时，每三位数自动添加一个逗号。你可能需要对一段长数字进行一些基本的数字格式化。

转换如下：

- 100 —> 100
- 1000 —> 1,000
- 1000000 —> 1,000,000

有三种相当简单的方法可以在 JavaScript 中执行此操作。

## Intl.NumberFormat

> [`Intl.NumberFormat`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Intl/NumberFormat) 是 JavaScript 中的一个构造函数，用于创建一个对象，该对象可用于将数字格式化为本地化的字符串。

例如，使用 `Intl.NumberFormat` 可以将数字格式化为不同地区的本地货币格式，如美元、欧元或人民币。

```js
const number = 123456.789

new Intl.NumberFormat('de-DE').format(number) // 123.456,789
new Intl.NumberFormat('ja-JP').format(number) // 123,456.789
new Intl.NumberFormat('en-IN', { maximumSignificantDigits: 3 }).format(number) // 1,23,000
```

你可以使用 `Intl.NumberFormat` 来定义要使用的货币、小数位数、数字分组分隔符、负号的位置等。

## Number.prototype.toLocaleString()

[`Number.prototype.toLocaleString()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number/toLocaleString) 方法返回这个数字在特定语言环境下的表示字符串。此方法也只是简单的调用 `Intl.NumberFormat`。

```js
const addCommas = (num) => num.toLocaleString()

addCommas(100) // '100'
addCommas(1000) // '1,000'
addCommas(1000000) // '1,000,000'
```

## 正则表达式

另一种选择是使用正则表达式。如果我们的输入是数字，我们将需要确保在应用正则表达式之前将其转换为字符串。

```js
const addCommas = (num) =>
  num.toString().replace(/(\d)(?=(\d{3})+(?!\d))/g, '$1,')

addCommas(100) // '100'
addCommas(1000) // '1,000'
addCommas(1000000) // '1,000,000'
```
