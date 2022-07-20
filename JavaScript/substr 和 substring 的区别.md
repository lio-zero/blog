# substr 和 substring 的区别

`substr` 和 `substring` 都是获取给定字符串的子字符串的常用方法。

## 区别

两种方法都有两个参数。第一个参数表示子字符串起始位置的索引。第二个参数是不同的。

```js
substr(startPosition, length)
substring(startPosition, endPosition)
```

如您所见，`substr` 和 `substring` 的第二个参数分别是子字符串的长度和结束位置。给定一个 `hello world` 字符串：

```js
'hello world'.substr(2, 4) // 'llo '
'hello world'.substring(2, 4) // 'll'
```

`substr` 允许使用负数作为起始位置参数。

```js
'hello world'.substr(-2, 4) // 'ld'
```

另一方面，子字符串将使负开始位置变为 0（零）：

```js
'hello world'.substring(-2, 5) // 'hello'
'hello world'.substring(0, 5) // 'hello'
```

## slice

`slice` 是获取子字符串的另一个方法。它没有被弃用为 [`substr`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/substr)，并且支持负索引。

```js
'hello world'.slice(2, 4) // 'll'
'hello world'.slice(-10, 5) // 'ello'
'hello world'.slice(-5) // 'world'
```
