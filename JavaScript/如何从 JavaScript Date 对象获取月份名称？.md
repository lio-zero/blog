# 如何从 JavaScript Date 对象获取月份名称？

我们用 JavaScript 中的当前日期时间创建一个 today 对象。

```js
let today = new Date()
console.log(today) // Wed May 05 2021 15:31:36 GMT+0800 (中国标准时间)
```

使用 `Date` 对象的内置 `getMonth` 方法获取当前月份作为值。

```js
let moonLanding = today.getMonth()
console.log(moonLanding) // 4
```

如果你想得到月份名称，可以自定义一个方法：

```js
const getMonthName = (val) => {
  const month = [
    'January',
    'February',
    'March',
    'April',
    'May',
    'June',
    'July',
    'August',
    'September',
    'October',
    'November',
    'December'
  ]
  return month[val]
}

console.log(getMonthName(moonLanding)) // "May"
```

也可以使用 `switch`，你觉得怎么优雅怎么来吧。

## 更好的方法

[`Date.prototype.toLocaleString()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/toLocaleString) 方法返回该日期对象的字符串，该字符串格式因不同语言而不同。

- 使用 `options` 参数来自定义 `toLocaleString` 方法返回的字符串。

```js
const today = new Date()

console.log(today.toLocaleString('default', { month: 'long' })) // "五月"
console.log(today.toLocaleString('en-GB', { month: 'long' })) // "May"
console.log(today.toLocaleString('ko-KR', { month: 'long' })) // "5월"
```

详细可以查看 [使用 `options` 参数](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Date/toLocaleString#example_using_options)

你也可以使用一些库来获取月份名称，如 [moment.js](https://momentjs.com/) 或 [day.js](https://dayjs.gitee.io/zh-CN/)。
