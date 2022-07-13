# 使用 JavaScript 的 Intl.DateTimeFormat() 构造函数转换和格式化日期和时间

## `Intl.DateTimeFormat()` 方法是如何工作的

Intl 对象的设计目的是使特定位置的数据更容易国际化。`DateTimeFormat()` 是一种用于格式化日期和时间的方法。

首先，创建一个新的 `Intl.DateTimeFormat()` 实例。

它接受一个 `locale`（例如，`zh-CN` 代表中文，`en-US` 代表美式英语，`en-GB` 代表英式英语）。此参数告诉方法将日期和时间格式化为哪种语言。

```js
const formatter = new Intl.DateTimeFormat('zh-CN')
const formatter = new Intl.DateTimeFormat('en-US')
const formatter = new Intl.DateTimeFormat('en-GB')
```

使用一系列 `options` 作为第二个参数，可以对输出的格式进行更细粒度的控制。

例如：以下示例使用 `dateStyle` 属性，然后使用 `format` 格式化为字符串：

```js
const now = new Date()

new Intl.DateTimeFormat('zh-CN', { dateStyle: 'full' }).format(now) // '2022年3月30日星期三'
new Intl.DateTimeFormat('zh-CN', { dateStyle: 'long' }).format(now) // '2022年3月30日'
new Intl.DateTimeFormat('zh-CN', { dateStyle: 'medium' }).format(now) // '2022年3月30日'
new Intl.DateTimeFormat('zh-CN', { dateStyle: 'short' }).format(now) // '2022/3/30'
new Intl.DateTimeFormat('en-US', { dateStyle: 'long' }).format(now) // 'March 30, 2022'

new Intl.DateTimeFormat('en-CA', { dateStyle: 'short' }).format(now) // '2022-03-30'
```

您可以查看 `options` 参数的完整列表，以及它们在 [Mozilla 开发者网络](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Intl/DateTimeFormat/DateTimeFormat#Syntax)上的功能。

## `Intl.DateTimeFormat()` 和 `Date.toLocaleString()` 有什么区别

`Date.toLocalString()` 方法的旧实现不支持 `locale` 或 `options` 参数，并且只使用当前用户的语言环境。

`Intl.DateTimeFormat()` 方法推出后，可选的 `options` 也被纳入 `Date.toLocaleString()`。

如果您使用相同的选项格式化多个日期，`Intl.DateTimeFormat()` 方法将为您提供更好的性能，应该是首选方法。否则，它们在行为上是相同的。

## 浏览器兼容性

`Intl.DateTimeFormat()` 方法适用于所有现代浏览器，也适用于 IE 11。

![Intl.DateTimeFormat](https://upload-images.jianshu.io/upload_images/18281896-e3f3ebd32a4fc106.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
