# JavaScript 中的数字截断

ES6 引入的 `Math.trunc()` 方法用于截断浮点数并返回其整数部分。此函数不进行任何舍入，它只是删除小数点后的所有数字。

```js
const number = 100.6

// 旧方法
number < 0 ? Math.ceil(number) : Math.floor(number) // 100

// ES6 方法
const es6 = Math.trunc(number) // 100
```

`Math.trunc()` 的运行方式很简单，它只是截断其右边的点和数字。不管参数是正数还是负数。

```js
Math.trunc(100.1) // 100
Math.trunc(100.12) // 100
Math.trunc(100.123) // 100

Math.trunc(-100.1) // -100
```

我们来看一些非数字参数的例子：

```js
Math.trunc('100.1') // 100
Math.trunc(null) // 0
Math.trunc('hello') // NaN
Math.trunc(NaN) // NaN
Math.trunc(undefined) // NaN
Math.trunc() // NaN
```

## 使用 `parseInt` 进行数字截断

我们使用 `parseInt` 也可以得到类似的结果：

```js
parseInt(100.1) // 100
parseInt(-100.1) // -100

parseInt('100.1') // 100
parseInt('hello') // NaN
parseInt(undefined) // NaN
parseInt(null) // NaN
parseInt() // NaN
```

## `Math.trunc()` 和 `parseInt()` 的选择

`parseInt` 主要用于字符串参数。所以如果你在处理数字，最好使用 `Math.trunc()`。

## 使用 `parseInt` 的问题

使用 `parseInt` 时可能会遇到问题。当您传入一个不是字符串的参数（在本例中是数字）时，它将首先使用 `toString()` 抽象操作将值转换为字符串。大多数情况下，`parseInt` 很好。但让我们看一个可能不是这样的例子。

```js
const number = 1000000000000000000000.5

parseInt(number) // 1 <-- 😱
```

为什么会出现这种情况？

这是因为我们的参数不是 `string`，所以 `parseInt` 做的第一件事就是将参数转换为 `string`。

```js
const number = 1000000000000000000000.5

console.log(number.toString()) // "1e+21"
```

所以当它试图从 `1e+21` 中获取整数时，它只知道获取 `1` 值。所以，使用 `parseInt` 肯定有其道理。但由于这种边缘情况，您可能需要考虑使用 `Math` 函数。

## 浏览器支持

大多数现代浏览器都支持 [`Math.trunc()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/trunc#Browser_compatibility)。除了 Internet Explorer。以下是 `Math.trunc()` 的支持情况：

![Math.trunc()](https://upload-images.jianshu.io/upload_images/18281896-5ede8f5d937f2ea1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

如果您需要对旧浏览器的支持，请使用旧方法。

## 其他解决方案

### 位运算符

**双位非 `~~`** — 用于去掉小数部分，因为位运算的操作值要求是整数，其结果也是整数，所以经过位运算的都会自动变成整数。

```js
console.log(~~100.6) // 100
```

一个 `~` 是按位取反运算，两个 `~~` 也就是取反两次。

**按位或 `|`** — 在每个位位置返回 `1`，其中一个或两个二进制操作数的对应位为 `1`。

```js
console.log(100.6 | 0) // 100
```

> 详细换算可查看 MDN：[Bitwise OR (|)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Bitwise_OR)。

### Number.prototype.toFixed()

`Number.prototype.toFixed()` 方法使用定点表示法来格式化一个数值，允许您控制小数。

```js
;(100.645).toFixed() // "100"

// 此方法返回一个字符串，但我们可以再次将其转换为数字
Number((100.645).toFixed()) // 100
```

使用 `toFixed` 方法需要注意一些舍入规则。详细内容可以查看现代 JS 教程的[数字类型](https://zh.javascript.info/number)章节。
