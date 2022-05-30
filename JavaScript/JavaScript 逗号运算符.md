# JavaScript 逗号运算符

[逗号操作符（`,`）](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Comma_Operator) 对它的每个操作数求值（从左到右），并返回最后一个操作数的值。

```js
let x = (2, 3)

console.log(x) // 3
```

虽然只返回最后一个操作数的值，但也有例外，你可以在其中执行自增运算符。

```js
let a = 0
let b = (a++, 11)
console.log(a) // 1
console.log(b) // 11
```

当放一个表达式时，它由左到右计算每个表达式，并传回最右边的表达式。

```js
const a = () => 'a'
const b = () => 'b'

const x = (a(), b())

console.log(x) // b
```

**注意**：逗号（`,`）操作符在 JavaScript 中所有的操作符里是最低的优先顺序，所以没有括号表达式将变为：`(x = a()), b()`。

其最多还是用在希望一个表达式的位置包含多个表达式。

```js
const arr = [1, 2, 3],
  str = 'hello' // 不用重复声明

console.log(arr) // [1, 2, 3]
console.log(str) // 'hello'
```

for 循环的多参数

```js
for (let i = 0, len = arr.length; i < len; i++) {}
```

我在之前写过一篇 [JavaScript 运算符总结](https://github.com/lio-zero/blog/blob/master/JavaScript/JavaScript%20%E8%BF%90%E7%AE%97%E7%AC%A6%E6%80%BB%E7%BB%93.md)，简单的总结了一些常用运算符的用法。
