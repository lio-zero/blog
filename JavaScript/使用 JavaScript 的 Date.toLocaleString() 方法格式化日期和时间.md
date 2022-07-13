# 使用 JavaScript 的 Date.toLocaleString() 方法格式化日期和时间

## `Date.toLocaleString()` 方法

[`Date.toLocaleString()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/toLocaleString) 方法可用于从 Date 对象创建格式化的日期和时间字符串。

它接受两个参数：格式化字符串的 `locale` 和格式化 `options` 对象。

`locale` 通常是一种语言代码。例如，对于中文，使用的是 `zh-cn`。`options` 对象包括 `dateStyle` 和 `timeStyle` 等设置（两者都接受 `full`、`long`、`middle` 和 `short` 的值）。

```js
let now = new Date()

// 获取格式化字符串
let formatDate = now.toLocaleString('en-US', {
  dateStyle: 'long',
  timeStyle: 'short',
  hour12: true
}) // '2022年3月29日 下午4:36'
```

## 格式因位置而异

`locale` 参数控制格式约定。

例如，使用英式英语（`en-UK`）或美式英语（`en-US`），我们的日期和时间返回格式略有不同。

```js
new Date().toLocaleString('en-UK', {
  dateStyle: 'long',
  timeStyle: 'short',
  hour12: true
}) // '29 March 2022, 04:40 pm'

new Date().toLocaleString('en-US', {
  dateStyle: 'long',
  timeStyle: 'short',
  hour12: true
}) // 'March 29, 2022 at 4:40 PM'
```

我们可以使用 `navigator.language` 属性将日期字符串自动格式化为用户的 `locale`。

```js
// 这将自动使用用户的浏览器语言进行格式化
new Date().toLocaleString(navigator.language, {
  dateStyle: 'long',
  timeStyle: 'short',
  hour12: true
}) // '2022年3月29日 下午4:41'
```
