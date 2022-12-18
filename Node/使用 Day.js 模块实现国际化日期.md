# 使用 Day.js 模块实现国际化日期

[Day.js](https://dayjs.gitee.io/zh-CN/) 是 [Moment.js](https://momentjs.com/) 的 2kB 轻量化方案，拥有同样强大的 API。

它提供了一系列用于处理日期的方法，且支持多语言，可以很容易地实现国际化日期。

本文将简单介绍一下它如何使用。

## 安装

NPM：

```js
npm i dayjs
```

CDN：

```html
<script src="https://unpkg.com/dayjs@1.8.21/dayjs.min.js"></script>
```

你也可以在 Day.js 官网的控制台测试该库提供的所有功能。

## 基本用法

引入 `dayjs`：

```js
import dayjs from 'dayjs'
```

[解析](https://day.js.org/docs/zh-CN/parse/parse)日期：

```js
const laborDay = dayjs('2022-5-1')

laborDay
/* M {
  '$L': 'en',
  '$d': 2022-04-30T16:00:00.000Z,
  '$x': {},
  '$y': 2022,
  '$M': 4,
  '$D': 1,
  '$W': 0,
  '$H': 0,
  '$m': 0,
  '$s': 0,
  '$ms': 0
} */
```

默认情况下，Day.js 使用美式英语（`en`）语言环境，而我们使用的语言是简体中文（`zh-CN`），要使用其他语言环境，可以使用 `locale` 获取或设置时间的国际化：

```js
// 你需要单独引入语言文件，如果不使用，它们不会包含在构建中
// 按需加载中文文件
require('dayjs/locale/zh-cn')

// 只在当前实例中使用
laborDay.locale('zh-cn') // 设置中文语言
// or：const laborDay = dayjs('2022-5-1', { locale: 'zh-cn' })
// 全局使用：dayjs.locale('zh-cn')

laborDay
/* M {
  '$L': 'zh-cn',
  ...
*/
```

[显示方法](https://day.js.org/docs/zh-CN/query/query)，例如 `format` 方法：

```js
laborDay.format('{YYYY} MM-DDTHH:mm:ss SSS [Z] A') // {2022} 05-01T00:00:00 000 Z 凌晨
laborDay.format('YYYY MMMM, dddd') // 2022 五月, 星期日
laborDay.format('YYYY-MM-DD') // 2022-05-01
```

> [查看所有可用格式的列表](https://github.com/iamkun/dayjs/blob/dev/docs/en/API-reference.md#list-of-all-available-formats)。

[取值和赋值方法](https://day.js.org/docs/zh-CN/get-set/get-set)，例如 `month` 和 `set` 方法：

```js
// 月份数为 0（1月份）- 11（12月份），
laborDay.month() // 4

laborDay.set('month', 5).month() // 5
```

[操作方法](https://day.js.org/docs/zh-CN/manipulate/manipulate)，例如 `add` 方法：

```js
laborDay.add(1, 'year').year() // 2023
```

[查询方法](https://day.js.org/docs/zh-CN/query/query)，例如 `isBefore` 和 `isAfter` 方法：

```js
laborDay.isBefore(dayjs()) // true
laborDay.isAfter(dayjs()) // false
```

## 高级用法

[插件](https://day.js.org/docs/zh-CN/plugin/plugin)是一个独立的模块，可以添加到 Day.js 以扩展功能或添加新特性。

例如，[`UTC` 插件](https://dayjs.gitee.io/docs/zh-CN/plugin/utc)添加了以 UTC 和本地时间获取日期的方法：

```js
const dayjs = require('dayjs')
const utc = require('dayjs/plugin/utc')

dayjs.extend(utc)

const laborDay = dayjs('2022-12-1')

// 将本地时间转换成 UTC 时间
dayjs.utc().format() // 2022-12-09T17:37:06Z

laborDay.utc().format() // 2022-11-30T16:00:00Z

// 注意这里返回的时间，UTC 返回的日期存在时区偏差，时间相对于本地时间相差八小时
```

关于国际化，我们可以使用 `AdvancedFormat`、`LocalizedFormat`、`RelativeTime` 和 `Calendar` 插件。

以下是 Day.js 文档 [`Calendar` 插件](https://dayjs.gitee.io/docs/zh-CN/plugin/calendar)的一个示例：

```js
const dayjs = require('dayjs')
const calendar = require('dayjs/plugin/calendar')

dayjs.extend(calendar)

console.log(
  dayjs().calendar(null, {
    sameDay: '[Today at] h:mm A', // The same day ( Today at 2:30 AM )
    nextDay: '[Tomorrow at] h:mm A', // The next day ( Tomorrow at 2:30 AM )
    nextWeek: 'dddd [at] h:mm A', // The next week ( Sunday at 2:30 AM )
    lastDay: '[Yesterday at] h:mm A', // The day before ( Yesterday at 2:30 AM )
    lastWeek: '[Last] dddd [at] h:mm A', // Last week ( Last Monday at 2:30 AM )
    sameElse: 'DD/MM/YYYY' // Everything else ( 17/10/2011 )
  })
)
// Today at 10:04 PM
```
