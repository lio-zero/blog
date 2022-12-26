# 检测 JavaScript 字符串中的表情符号

表情符号通常由 Unicode 编码的表情符号字符组成，例如 😄 笑脸。

通过一些特殊的正则表达式，我们可以检测 JavaScript 中的字符串是否包含表情符号：

```js
function hasEmoji(str) {
  const emojiRegex =
    /([\uE000-\uF8FF]|\uD83C[\uDC00-\uDFFF]|\uD83D[\uDC00-\uDFFF]|[\u2694-\u2697]|\uD83E[\uDD10-\uDD5D])/g
  return emojiRegex.test(str)
}

hasEmoji('Hello 😄 World') // true
```

但这并不通用，上述正则表达式可能不能匹配所有可能的表情符号，因为表情符号的数量和类型会不断变化。如果需要更全面的支持，需要使用更复杂的正则表达式或其他方法来匹配表情符号。

我们有一种更简便的方法来检查字符串中的表情符号。

> [MDN 描述了 Unicode 模式将正则表达式模式视为 Unicode 代码点序列](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions#advanced_searching_with_flags)而不是代码单元。

在正则表达式中启用 Unicode 模式时，还可以使用 Unicode 属性转义。Unicode 属性转义（`\p{}` 或 `\P{}`）允许你根据 Unicode 字符的属性和特征匹配它们。

```js
const emojiRegex = /\p{Emoji}/u
emojiRegex.test('⭐') // true

// 大写字母 P 否定了匹配
const noEmojiRegex = /\P{Emoji}/u
noEmojiRegex.test('⭐') // false
```

因此，我们可以将一开始的函数更改为：

```js
function hasEmoji(str) {
  const emojiRegex = /\p{Emoji}/u
  return emojiRegex.test(str)
}

hasEmoji('Hello 😂 World') // true
```

如果你想替换和修改 JavaScript 字符串中的表情符号，你可以使用 `String.replaceAll` 方法。

```js
// 注意 g 标志以替换所有表情符号
'🙈-👍-⭐'.replaceAll(/\p{Emoji}/gu, '_') // '_-_-_'
```
