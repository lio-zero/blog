# 检测 JavaScript 字符串中的表情符号

> [MDN 描述了 Unicode 模式将正则表达式模式视为 Unicode 代码点序列](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions#advanced_searching_with_flags)而不是代码单元。

在正则表达式中启用 Unicode 模式时，还可以使用 Unicode 属性转义。Unicode 属性转义（`\p{}` 或 `\P{}`）允许您根据 Unicode 字符的属性和特征匹配它们。

```js
const emojiRegex = /\p{Emoji}/u
emojiRegex.test('⭐') // true

// 大写字母 P 否定了匹配
const noEmojiRegex = /\P{Emoji}/u
noEmojiRegex.test('⭐') // false
```

如果你想替换和修改 JavaScript 字符串中的 Emojis，你可以使用 `String.replaceAll`。

```js
// 注意 g 标志以替换所有 Emojis
'🙈-👍-⭐'.replaceAll(/\p{Emoji}/gu, '_') // '_-_-_'
```
