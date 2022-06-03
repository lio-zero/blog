# 如何在 JavaScript 中交换两个变量？

过去，在 JavaScript 中交换两个变量的值需要一个中间变量在交换时存储其中一个值

```js
let a = 10
let b = 20

let temp
temp = a
a = b
b = temp
```

尽管这种方法仍然有效，但如今有更多更优雅供我们使用。

例如，JavaScript ES6 引入了解构赋值，允许在单个语句中将单个数组项分配给变量。看起来像这样：

```js
const [x, y] = [1, 2]
```

解构赋值在少数情况下非常有用，包括交换两个变量。为此，我们可以从两个变量创建一个数组，然后使用解构分配将它们彼此重新分配：

```js
let a = 10
let b = 20

[a, b] = [b, a]
```

## 其他解决方案

您可以在 [How to swap two variables in JavaScript](https://stackoverflow.com/questions/16201656/how-to-swap-two-variables-in-javascript) 找到更多的解决方法。

例如，我们还可以使用[按位异或运算符](http://en.wikipedia.org/wiki/XOR_swap_algorithm)：

```js
a = a ^ b
b = a ^ b
a = a ^ b
```

但使用这些技巧或多或少都有一些限制，像上面这种方式，只适用于数字类型的整数值。
