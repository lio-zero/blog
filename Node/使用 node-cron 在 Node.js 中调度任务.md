# 使用 node-cron 在 Node.js 中调度任务

没有一个开发人员愿意把所有时间都花在繁琐的任务上，比如系统维护和管理、日常数据库备份以及定期下载文件和电子邮件。你更愿意专注于富有成效的工作，而不是跟踪这些烦人的琐事何时需要完成。

这时就需要使用到**任务调度**，它将帮助您解决这样的问题。

**任务调度**使您能够计划任意代码（方法/函数）和命令在固定日期和时间、重复间隔或指定间隔后执行一次。在 Linux 操作系统中，任务调度通常由诸如 cron 之类的实用程序服务在操作系统级别处理。

在 Node.js 应用程序中，类似于 cron 的功能，我们可以使用 [node-cron](https://github.com/kelektiv/node-cron) 这样的包实现。正如开发者所介绍的，**[node-cron](https://www.npmjs.com/package/node-cron)** 是基于 GNU crontab 的 node.js 纯 JavaScript 中的微型任务调度器。

crontab 是 Linux 系统的定时任务执行器。cron 的操作由 [crontab](https://www.gnu.org/software/mcron/manual/html_node/Crontab-file.html) 文件驱动，该文件是一个配置文件，其中包含对 cron 守护程序的指令。`node-cron` 模块允许我们使用完整的 crontab 语法在 Node 中调度任务。

> 🎉**推荐工具**
>
> [crontab 编辑器](https://crontab.guru/)：在线工具可以可视化生成 crontab 的配置文件。

crontab 语法如下所示：

```bash
 # ┌────────────── second (可选)
 # │ ┌──────────── 分钟 (minute，0 - 59)
 # │ │ ┌────────── 小时 (hour，0 - 23)
 # │ │ │ ┌──────── 一个月中的第几天 (day of month，1 - 31)
 # │ │ │ │ ┌────── 月份 (month，1 - 12)
 # │ │ │ │ │ ┌──── 星期中星期几 (day of week，0 - 6) 注意：星期天为 0
 # │ │ │ │ │ │
 # │ │ │ │ │ │
 # * * * * * *
```

允许的 cron 值包括以下内容。

| **字段**           | **值**                                 |
| ------------------ | -------------------------------------- |
| `second`           | 0-59                                   |
| `minute`           | 0-59                                   |
| `hour`             | 0-23                                   |
| `day of the month` | 1-31                                   |
| `month`            | 1-12（或月份简写 Jan、Feb...）         |
| `day of the week`  | 0-7（或 Jan、Feb...，0 或 7 是星期日） |

下面我们来看看它的一些用法和用例。

## 使用 node-cron

使用 `npm` 安装 `node-cron` 模块。

```bash
npm install --save node-cron
```

### 语法

```js
cron.schedule(cronExpression: string, task: Function, options: Object)
```

### 选项

- `scheduled`：一个布尔值（`boolean`），用于设置创建的任务是否已安排（默认值为 `true`）。
- `timezone`：用于任务调度的时区。有关有效值，可参考 [moment-timezone](https://momentjs.com/timezone)。

看看下面的例子。

```js
const cron = require('node-cron')

cron.schedule('5 * * * * *', () => {
  console.log('每分钟在第 5 秒运行一个任务')
})
```

时间规范的位置 2、3、4、5 和 6 中的星号（`*`）类似于用于时间划分的文件 glob 或通配符；它们分别指定**每分钟**、**每小时**、**每月的每一天**、**每月和每周的每一天**。

以下代码将在每天凌晨 5:30 运行。

```js
const cron = require('node-cron')

cron.schedule('30 5 * * *', () => {
  console.log('每天凌晨 5:30 运行任务')
})
```

## 任务调度提示和技巧

现在我们已经了解了基本知识，让我们做一些更有趣的事情。

假设您希望在每周五下午 4 点运行一项特定任务。代码如下所示：

```js
const cron = require('node-cron')

cron.schedule('0 16 * * friday', () => {
  console.log('每周五下午 4:00 运行任务')
})
```

或者，您可能需要每季度运行一次数据库备份。crontab 语法没有**一个月的最后一天**选项，因此您可以使用**下个月的第一天**，如下所示。

```js
const cron = require('node-cron')

cron.schedule('2 3 1 1,4,7,10 *', () => {
  console.log('在每个季度的第一天运行任务')
})
```

下面显示的任务在上午 10:05 到下午 6:05 之间每小时运行五分钟。

```js
const cron = require('node-cron')

cron.schedule('5 10-18 * * *', () => {
  console.log('在上午 10 点到下午 6 点之间每小时运行五分钟的任务')
})
```

在某些情况下，您可能需要每两小时、三小时或四小时运行一次任务。您可以通过将小时数除以所需的时间间隔来完成此操作，例如，每四小时 `*4`，或在上午 12 点到下午 12 点之间每三小时运行 `0-12/3`。

分钟也可以用同样的方法划分。例如，`minutes` 位置的表达式为 `*/10`，表示**每 10 分钟运行一次任务**。

下面的任务在上午 8 点到下午 5:58 之间每两小时运行五分钟。

```js
const cron = require('node-cron')

cron.schedule('*/5 8-18/2 * * *', () => {
  console.log('在上午 8 点到下午 5:58 之间每两小时运行一次任务。')
})
```

## 定时任务方法

在结束之前，让我们关注一下三个关键的定时任务方法。

### 开始任务

将 `scheduled` 选项值设置为 `false` 时，任务将被调度，但无法启动，即使 cron 表达式正在滴答作响。

要启动这样的任务，您需要调用 `start` 方法。

```js
const cron = require('node-cron')

const task = cron.schedule('*/5 8-18/2 * * *', () => {
  console.log('在上午 8 点到下午 5:58 之间每两小时运行一次任务。')
})

task.start()
```

### 停止任务

如果需要停止任务运行，可以使用 `stop` 方法将 `scheduled` 选项设置为 `false`。除非重新启动，否则不会执行该任务。

```js
const cron = require('node-cron')

const task = cron.schedule('*/5 8-18/2 * * *', () => {
  console.log('在上午 8 点到下午 5:58 之间每两小时运行一次任务。')
})

task.stop()
```

### 销毁任务

`destroy` 方法停止任务并将其完全销毁。

```js
const cron = require('node-cron')

const task = cron.schedule('*/5 8-18/2 * * *', () => {
  console.log('在上午 8 点到下午 5:58 之间每两小时运行一次任务。')
})

task.destroy()
```

以上便是 `node-cron` 的大部分功能，您应该使用这些功能来安排频繁运行的任务。
