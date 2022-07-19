# Node.js readline 模块

[`readline` 模块](https://nodejs.org/api/readline.html#readline)提供了一种读取数据流的方法，一次一行。

它有两种 API：

```js
// 基于 promise 的 API
const readline = require('readline/promises')
// 回调和同步的 API
const readline = require('readline')
```

以下示例是 `readline` 模块的基本用法：

```js
const readline = require('readline')
const { stdin: input, stdout: output } = require('process')

const rl = readline.createInterface({ input, output })

rl.question('你觉得 Node.js 怎么样？', (answer) => {
  console.log(`感谢您的宝贵反馈: ${answer}`)

  rl.close()
})
```

步骤如下：

- 使用 `readline` 的 `createInterface` 方法创建了一个接口实例
- 调用实例的[相关方法](https://nodejs.org/api/readline.html#class-interfaceconstructor)，如 `question` 方法输入
- 监听 `readline` 的 `close` 事件

一旦执行，Node.js 应用程序将不会终止，直到 `readline.Interface` 关闭，因为接口在输入流上等待接收数据。

当所有操作的完成时，我们使用 `close` 方法来结束程序。`close` 方法会触发 `close` 事件：

```js
rl.on('close', () => {
  console.log('退出程序')
  process.exit(0)
})
```

## `readline` 属性和方法

- `clearLine()` 清除指定流的当前行
- `clearScreenDown()` 从当前光标向下位置清除指定的流
- `createInterface()` 创建一个接口对象
- `cursorTo()` 将光标移动到指定位置
- `emitKeypressEvents()` 为指定流触发按键事件
- `moveCursor()` 将光标移动到相对于当前位置的新位置

详细 API 请查阅[文档](https://nodejs.org/api/readline.html#readline)。

## 示例：打开一个文件并逐行返回内容

使用 [`line` 事件](https://nodejs.org/api/readline.html#event-line)，逐行读取文件内容：

```js
const readline = require('readline')
const fs = require('fs')

const rl = readline.createInterface({
  input: fs.createReadStream('./package.json')
})

let lineno = 0
rl.on('line', (line) => {
  lineno++
  console.log(`Line number ${lineno}': ${line}`)
})
```

## 示例：回文扫描器

```js
const readline = require('readline')

const { stdin: input, stdout: output } = require('process')
const rl = readline.createInterface({
  input,
  output
})

function isPalindrome(str) {
  const result = str
    .replace(/[\W_]/g, '')
    .toLowerCase()
    .split('')
    .reverse()
    .join('')
  if (result == str.toLowerCase()) return true
  else return false
}

console.log('------- 回文扫描器 ------- ')
console.log('输入回文: ')
rl.on('line', (input) => {
  console.log(`${input} ${isPalindrome(input) ? '是' : '不是'}回文`)
  console.log('输入回文: ')
})
```
