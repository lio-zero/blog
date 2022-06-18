# 如何在 Node.js 中控制没有依赖项的日志消息

数以百万计的项目包依赖于非常流行的 [`debug`](https://www.npmjs.com/package/debug) 包。提供的 `debug` 方法使 Node.js 开发人员能够控制日志消息传递。与常使用的 `console.log` 相反，使用 `debug` 的消息在默认情况下是隐藏的。

`debug` 日志消息被绑定到一个模块名，只有当 `debug` 环境变量列出特定模块名时才会出现。

```js
// 只有当设置 DEBUG=http 时记录消息
const debug = require('debug')('http')

debug('booting %o', name)
```

## `util.debuglog` —原生的 `debug` 备选方案

Node.js 内置了类似的功能。[`util.debuglog`](https://nodejs.cn/api/util.html#util_util_debuglog_section) 方法提供了几乎相同的功能。

让我们看一个示例：

```js
// index.js
const util = require('util')
const debuglog = util.debuglog('app')

debuglog('hello from my debugger [%d]', 123)
```

在终端中运行此代码时，将不会看到任何日志消息。但是，当您定义在 `app` 日志消息中进行测试并定义环境变量 `NODE_ DEBUG=app` 时，日志消息将显示：

```bash
$ NODE_DEBUG=app node index.js
APP 1000: hello from my debugger [123]
```

`util.debuglog` 甚至支持通配符（`*`），以防您希望同时为不同的模块启用日志消息。

```js
// index.js
const util = require('util')
const logGeneral = util.debuglog('app-general')
const logTimer = util.debuglog('app-timer')
const delay = 500

logGeneral('启动应用程序')

setTimeout(() => {
  logTimer('%d 秒后计时器启动', delay)
}, delay)
```

使用 `app-*` 环境变量运行脚本会导致以下结果：

```bash
$ NODE_DEBUG=app-* node index.js
APP-GENERAL 7384: 启动应用程序
APP-TIMER 7384: 500 秒后计时器启动
```

`NODE_DEBUG` 环境变量还可以用于从 Node.js 内部获取调试消息。您可能偶尔会在 Node.js 文档中遇到它。

## 最后

了解 `util.debuglog` 很好，但它没有涵盖 `debug` 包的所有功能。对我来说，`util.debuglog` 是小型项目中 `debug` 包的一个很好的替代方案。
