# JavaScript 获取字符串的最后一个字符

- 使用 `String.prototype.split()` 将字符串转为数组，在访问其下标获取字符。

```js
let str = 'Hello'
let arr = str.split('')
let last_character = arr[arr.length - 1]

console.log(last_character) // "o"
```

- [`String.prototype.charAt()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/charAt) 方法从一个字符串中返回指定的字符。

```js
let last_character = str.charAt(str.length - 1)

console.log(last_character) // "o"
```

- [`String.prototype.substr()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/substr) 方法返回一个字符串中从指定位置开始到指定字符数的字符。

```js
let last_character = str.substr(str.length - 1, 1) // "o"

console.log(last_character) // "o"
```

> **注意**：此方法没有严格被废弃，但应该避免使用它，当做了解就行。可以使用 [`String.prototype.substring()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/substring) 代替。

```js
let last_character = str.substring(str.length - 1) // "o"

console.log(last_character) // "o"
```

- 使用 [`String.prototype.replace()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/replace) 方法，传递一个正则表达式，`$2` 匹配结果中对应的分组来获取最后一个字符。

```js
const last_character = str.replace(/^(.*[n])*.*(.|n)$/g, '$2')

console.log(last_character) // "o"
```
