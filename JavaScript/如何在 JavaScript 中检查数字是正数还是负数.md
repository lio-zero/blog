# 如何在 JavaScript 中检查数字是正数还是负数

ES6 的 [`Math.sign`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/sign#Browser_compatibility) 方法返回一个数字的符号，指示数字是正数，负数还是零。

使用起来很简单：

```js
Math.sign(6) // 1
Math.sign(-6) // -1
Math.sign(0) // 0
```

## `Math.sign` 返回值

`Math.sign()` 有 5 个可能的返回值：

```js
1  // 正数
-1 // 负数
0  // 正零
-0 // 负零
NaN // NaN 不是数字

Math.sign(8) // 1
Math.sign(-8) // -1

Math.sign(0) // 0
Math.sign(-0) // -0

Math.sign(NaN) // NaN
Math.sign('hello') // NaN
Math.sign() // NaN
```

> **注意**：传递给此方法的参数将隐式转换为 `number` 类型。

一个常见的错误是认为 `Math.sign` 返回转换后的参数值。而事实上，`Math.sign` 只返回数字的符号，它不返回值。

```js
Math.sign(-8) // -1
```

可以看到，它返回了 `-1` 而不是返回 `-8`。

## `Math.sign` 与比较运算符

> 有人可能会说：既然我能用比较运算符，为什么还要用 `Math.sign`？

```js
const number = 1
// 比较运算符
if (number > 0) // 正
else // 负

// 三元运算符
return (number < 0) ? -1 : (number > 0) ? 1 : 0

// Math.sign
if (Math.sign(number) > 0) // 正
else // 负
```

实际上，如果您只是检查布尔状态，那么我们可以使用比较运算符，而不是使用 `Math.sign`。

但我们要知道 `Math.sign` 的作用是返回一个数值。这意味着你可以做计算。

```js
const number = 5

number > 0 // true
Math.sign(number) // 1
```

> 且 `Math.sign()` 相比比较/三元运算符来说，可读性更高，编写、阅读、理解方面所花费的时间更少。

### `Math.sign` 解决算法问题

我们可以使用 `Math.sign` 来解决**整数反转**算法：

> 123 ——> 321
>
> -123 ——> -321

```js
const reverseInteger = (num) => {
  const numArray = Math.abs(num).toString().split('').reverse().join('')

  const sign = Math.sign(num) // -1
  return numArray * sign
}

reverseInteger(-321) // -123
```

> 你可以在 LeetCode 的 [整数反转](https://leetcode-cn.com/problems/reverse-integer/)进行练习。
>
> 推荐：[JavaScript 反转字符串](https://github.com/lio-zero/blog/blob/main/JavaScript/JavaScript%20%E5%8F%8D%E8%BD%AC%E5%AD%97%E7%AC%A6%E4%B8%B2.md)

## 负零

`Math.sign` 还返回负零：

```js
Math.sign(0) // 0
Math.sign(-0) // -0
```

为什么返回负零，我们需要它吗 🤨？存在就有它存在的道理，[《You Don't Know JS》](https://github.com/getify/You-Dont-Know-JS)中的 Kyle Simpson 解释得非常好：

> Now, why do we need a negative zero, besides academic trivia?
>
> There are certain applications where developers use the magnitude of a value to represent one piece of information (like speed of movement per animation frame) and the sign of that number to represent another piece of information (like the direction of that movement).
>
> In those applications, as one example, if a variable arrives at zero and it loses its sign, then you would lose the information of what direction it was moving in before it arrived at zero. Preserving the sign of the zero prevents potentially unwanted information loss.

译文如下：

> **现在，除了学术琐事，我们为什么还需要负零呢？**
>
> 在某些应用中，开发人员使用数值的大小来表示一条信息（如每个动画帧的移动速度），并使用该数字的符号来表示另一条信息（如移动的方向）。
>
> 在这些应用中，举个例子，如果一个变量到达零，并且它失去了符号，那么在它到达零之前，您将丢失它正在向哪个方向移动的信息。保留零的符号可以防止潜在的不必要的信息丢失。

## `Math.sign` 浏览器支持情况

除了 IE，`Math.sign` 支持所有的主流的浏览器，支持情况如下图所示：

![Math.sign 浏览器支持](https://upload-images.jianshu.io/upload_images/18281896-b544534ef3eafd02.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

如果你需要在 IE 或旧浏览器中使用它，你可以使用 MDN 提供的 [Polyfill](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/sign#Polyfill)：

```js
if (!Math.sign) {
  Math.sign = function (x) {
    return (x > 0) - (x < 0) || +x
  }
}
```

也可以选择其他解决方法：

```js
const positive = 5
const negative = -5
const zero = 0

positive === 0 ? positive : positive > 0 ? 1 : -1 // 1
negative === 0 ? negative : negative > 0 ? 1 : -1 // -1
zero === 0 ? zero : zero > 0 ? 1 : -1 // 0
```

[core-js](https://github.com/zloirock/core-js#ecmascript-math) 也提供了相应的 polyfill。
