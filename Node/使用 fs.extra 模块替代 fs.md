# 使用 fs.extra 模块替代 fs

[`fs-extra`](https://github.com/jprichardson/node-fs-extra) 是原生 `fs` 的替代品。该模块继承了 `fs-extra` 中所有方法，添加了原生 `fs` 模块中不包含的文件系统方法，并向 `fs` 方法添加了 promise 支持。

## 基本用法

安装：

```js
npm i fs-extra
```

引入：

```js
const fse = require('fs-extra')
```

`fs-extra` 提供的每个方法都有同步和异步版本，例如：

```js
const { copy, copySync } = require('fs-extra')

// 同步
try {
  copySync('/tmp/myFile', '/tmp/myNewFile')
  console.log('success!')
} catch (err) {
  console.error(err)
}

// 异步 promise
copy('/tmp/myFile', '/tmp/myNewFile')
  .then(() => console.log('success!'))
  .catch((err) => console.error(err))

// 异步回调
copy('/tmp/myFile', '/tmp/myNewFile', (err) => {
  if (err) return console.error(err)
  console.log('success!')
})
```

异步方法还可以使用 `async/await`：

```js
async function copyFiles() {
  try {
    await copy('/tmp/myFile', '/tmp/myNewFile')
    console.log('success!')
  } catch (err) {
    console.error(err)
  }
}

copyFiles()
```

## 清空文件夹

使用 `emptyDirSync()` 方法清空文件夹的所有文件：

```js
const { emptyDirSync } = require('fs-extra')

const folder = './node_modules'

emptyDirSync(folder)
```

> [点击此处查看所有的 API 示例](https://github.com/jprichardson/node-fs-extra/tree/0220eac966d7d6b9a595d69b1242ab8a397fba7f/docs)
