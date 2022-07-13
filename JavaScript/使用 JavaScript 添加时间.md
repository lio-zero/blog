# 使用 JavaScript 添加时间

当前在 JavaScript 中管理日期和时间的方法是使用 `Date()` 对象。还行，但不太好。

它具有从字符串创建 `Date()` 对象、格式化日期和时间以及取回特定值的方法。但它缺乏从日期中添加或减去时间的原生方法。时区支持也很糟糕。

有一个新的 API 正在开发中：[`Temporal()`](https://tc39.es/proposal-temporal/docs/)。它将允许您执行以下操作：

```js
// 目前浏览器中不支持这一点。
Temporal.Now.instant().add({ hours: 5, seconds: 20 })
```

它目前是第 3 阶段的提案，这意味着它与标准流程相距甚远。

那么，我们先来看看 `Date()` 的一些用法。

## 创建 `Date()` 对象

您可以使用 `new Date()` 构造函数创建 `Date()` 对象。

如果不传入任何参数，它将为当前运行的时刻创建一个 `Date()` 对象。也可以将日期字符串作为参数传入。

结果 `Date()` 对象与您当前的时区相关。

```js
// 现在的日期对象
let now = new Date()

// 国庆节日期对象
let nationalDay = new Date('October 1, 2022')
```

您还可以通过传入一系列参数来创建日期：`year`、`monthIndex`、`day`、`hours`、`minutes`、`seconds` 和 `milliseconds`。只有 `year` 和 `monthIndex` 是必需的。

`monthIndex` 参数让人感到困惑，因为它从 1 月的 `0` 开始，而不是 `1`。

```js
// 请注意，尽管 10 月是第 10 个月，但月份指数是 9
let halloween = new Date(2022, 10, 1)

// 当地时间国庆节上午 10:00
let nationalDay = new Date(2022, 10, 1, 10, 00)
```

十二月份的月份数为 0：

```js
let December = new Date(2022, 10)
December.getHours() // 0
```

它很笨重，但很实用。

## 修改日期

`Date()` 对象提供了一些实例方法，可以用来获取和修改日期和时间。它们遵循 `get*()` 和 `set*()` 命名约定，其中 `*` 是要获取或设置的属性。

例如，要获取 `nationalDay` 的 `hours` 和 `month` 属性，使用 `getHours()` 和 `getMonth`。

```js
nationalDay.getHours() // 10

// 记住：它从 0 开始
nationalDay.getMonth() // 10
```

要设置 `hours` 和 `month` 属性，使用 `setHours()` 和 `setMonth`，传入想要设置的时间参数即可。它使用 24 小时制的时钟。

```js
// 将时间设置为下午 2 点，14 小时
nationalDay.setHours(14)

// 把月份定在 7 月
nationalDay.setMonth(6)
```

## 添加和删除时间

现在，为了添加和删除时间，我们可以结合使用 `get*()` 和 `set*()` 方法。

例如，要向一个时间添加 4 小时，可以使用 `getHours()` 方法获取当前的小时数，再向其中添加 4 小时，然后将结果传递给 `setHours()` 方法。

```js
// 将时间设置为下午 6 点（下午 2 点 +4 小时）
nationalDay.setHours(nationalDay.getHours() + 4)
```

如果由此产生的调整会让您进入下一天、下个月、下一年等，那么 `Date()` 对象会自动考虑这一点并正确地进行调整。

```js
// 将日期设置为 12 月 26 日凌晨 2 点（12 月 25 日下午 6 点 +8 小时）
nationalDay.setHours(nationalDay.getHours() + 8)
```

## 格式化日期

修改日期和时间后，可以将其格式化为字符串。

有两种方法：[`Date.toLocaleString()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/toLocaleString) 方法和 [`Intl.dateTimeFormat()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Intl/DateTimeFormat/DateTimeFormat) 方法。

```js
nationalDay.toLocaleString(navigator.language, {
  dateStyle: 'full',
  timeStyle: 'short',
  hour12: true
}) // '2022年7月2日星期六 上午2:00'

new Intl.DateTimeFormat(navigator.language, {
  dateStyle: 'full',
  timeStyle: 'short',
  hour12: true
}).format(nationalDay) // '2022年7月2日星期六 上午2:00'
```

它们都支持相同的格式选项。`Date.toLocaleString()` 方法较旧，但使用起来更容易一些。如果您使用相同的设置格式化大量日期，那么 `Intl.dateTimeFormat()` 方法方法的性能会更好。
