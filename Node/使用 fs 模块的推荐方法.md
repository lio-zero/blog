# 使用 fs 模块的推荐方法

在编写使用 `fs` 模块的 Node.js 脚本时，我通常使用 `util.promisify` 方法来保证文件系统方法。基于 Promise 的方法允许使用 `async/await`，这使得代码更容易掌握和阅读。

从 Node.js 11 开始，`fs` 模块在 [`promises` 属性中提供了 `promisify` 方法](https://nodejs.org/api/fs.html#fs_fs_promises_api)。

```js
// 旧的方法使用 util.promisify
const { readFile } = require('fs')
const { promisify } = require('util')
const promisifiedReadFile = promisify(readFile)

promisifiedReadFile(__filename, { encoding: 'utf8' }).then((data) =>
  console.log(data)
)

// 使用基于 promise 的 fs 的新方法
const { readFile } = require('fs').promises
readFile(__filename, { encoding: 'utf8' }).then((data) => console.log(data))
```

使用 `promises` 属性，您现在可以跳过将回调转换为 promise 的步骤，无需使用 `util.promisify`。这对于扁平化一些源代码并使用 `async/await` 是个好消息！

## `fs/promises` 从 Node.js 14 开始可用

从 Node.js 14 开始，`fs` 模块提供了两种使用基于 promises 的文件系统的方法。这些 promises 可以通过 `require('fs').promises` 或 `require('fs/promises')` 获得。

```js
// 从 Node.js v14 开始：使用基于 Promise 的 fs 方法
const { readFile } = require('fs/promises')

readFile(__filename, { encoding: 'utf8' }).then((data) => console.log(data))
```

Node.js 的维护者们对于 `/promises` 路径方式来公开更多现有模块的基于 promise 的方法表示赞成。

后面的 Node.js 版本也遵循了这种做法。

## 更多资料

[Node.js 文件系统模块](https://github.com/lio-zero/blog/blob/master/Node/Node.js%20%E6%96%87%E4%BB%B6%E7%B3%BB%E7%BB%9F%E6%A8%A1%E5%9D%97.md)
