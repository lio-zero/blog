# JavaScript 反转字符串

今天，我们来写一个反转字符串的函数。它将实现如下所示：

```txt
"abcdefg" -> "gfedcba"
```

## Array.prototype.reverse()

最简便的操作是使用 `Array.prototype.reverse()` 方法，它将数组中元素的位置颠倒，并返回该数组。该方法会改变原数组。

- 使用 `String.prototype.split()` 方法将字符串转换为数组。
- 使用 `Array.prototype.reverse()` 方法将其反转，然后使用 `Array.prototype.join()` 方法将其转换回字符串

```js
const reverseString = (str) => str.split('').reverse().join('')

console.log(reverseString('abcdefg')) // "gfedcba"
```

## 循环

创建一个空字符串，它将保存反向字符串，循环遍历字符串中的每个字符，并将其附加到新字符串的开头。

```js
const reverseString = (str) => {
  let result = ''

  for (let i = str.length - 1; i >= 0; i--) {
    result += str[i]
  }

  return result
}

console.log(reverseString('abcdefg')) // "gfedcba"
```

使用 `for ... of`

```js
const reverseString = (str) => {
  let result = ''

  for (let character of str) {
    result += character
  }

  return reverseString
}

reverseString('abcdefg') // "gfedcba"
```

## Array.prototype.reduce()

[`Array.prototype.reduce()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reverse) 方法对数组中的每个元素执行一个由您提供的 **reducer** 函数（升序执行），将其结果汇总为单个返回值。

- 使用 `String.prototype.split()` 方法将字符串转换为数组。也可以使用扩展运算符，**看你**。
- 使用 `Array.prototype.reduce()` 方法将其转换为字符串。

```js
const reverseString = (str) =>
  str.split('').reduce((rev, char) => char + rev, '')

console.log(reverseString('abcdefg')) // "gfedcba"
```

也可以使用 `Array.prototype.reduceRight()`

```js
const reverseString = (str) => [...str].reduceRight((acc, cur) => acc + cur)

reverseString('abcdefg') // "gfedcba"
```

## Array.prototype.sort

使用 `Array.prototype.sort` 反转字符串，这里不过多介绍，跟上面其他方法差不多，你可以再 [如何在 JavaScript 中对对象数组进行排序？](https://github.com/lio-zero/blog/blob/master/JavaScript/%E5%A6%82%E4%BD%95%E5%9C%A8%20JavaScript%20%E4%B8%AD%E5%AF%B9%E5%AF%B9%E8%B1%A1%E6%95%B0%E7%BB%84%E8%BF%9B%E8%A1%8C%E6%8E%92%E5%BA%8F%EF%BC%9F.md) 查看 sort 如何使用。

```js
function reverseString(str) {
  return str
    .split('')
    .sort(() => -1)
    .join('')
}

reverseString('abcdefg') // "gfedcba"
```

## 递归

递归是通过使用调用自身的函数来解决问题的一种方法。每次函数调用自身时，它都会将问题简化为子问题。此递归调用将继续，直到到达无需进一步递归即可调用子问题的点为止。

> **注意**：给予递归正确的终止条件，以确保不会导致无限循环。

- 递归终止的条件为字符串为空，将停止递归。
- 使用 [`String.prototype.substring()`](https://dev.to/sloan/explain-recursion-like-im-five-5c6) 方法获取并删除字符串中的第一个字符，然后将其他字符传递给函数。在将第一个字符附加到 `return` 语句中- 。

```js
const reverseString = (str) =>
  str ? reverseString(str.substring(1)) + str[0] : str

reverseString('abcdefg') // "gfedcba"
```
