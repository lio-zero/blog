# 使用 Node.js 递归创建目录

最近，在处理一个新项目时，我需要创建一系列嵌套目录。从命令行，这很简单，只需将 `-p` 传递给 `mkdir`，它就会自动创建所有父目录。就像这样

```bash
$ mkdir -pv ./path/to/my/directory

# mkdir: created directory './path'
# mkdir: created directory './path/to'
# mkdir: created directory './path/to/my'
# mkdir: created directory './path/to/my/directory'
```

使用 Node.js 的现代版本，特别是 10 及以上版本，您可以使用 `fs` 实现相同的功能：

## 同步

```js
const fs = require('fs')

fs.mkdirSync('./path/to/my/directory', { recursive: true })
```

## 异步

```js
const fs = require('fs')

await fs.promises.mkdir('./path/to/my/directory', { recursive: true })
```

假设您使用的是 Node.js 的旧版本，并且不愿意升级到 10+，那么您将不得不手动执行更多操作。

要点是，您需要将路径分开并循环遍历其中的每个部分，同时创建不存在的父目录。看起来像这样：

```js
const fs = require('fs')

const path = './path/to/my/directory'

path.split('/').reduce((directories, directory) => {
  directories += `${directory}/`

  if (!fs.existsSync(directories)) {
    fs.mkdirSync(directories)
  }

  return directories
}, '')
```

`fs.existsSync()` 将判断目录是否存在，如果该目录不存在，使用 `fs.mkdirSync()` 以同步的方式创建目录。

## 更多资料

[Node.js 文件系统模块](https://github.com/lio-zero/blog/blob/master/Node/Node.js%20%E6%96%87%E4%BB%B6%E7%B3%BB%E7%BB%9F%E6%A8%A1%E5%9D%97.md)
