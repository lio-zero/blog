# 在 Node.js 中使用 execa 运行命令

Node.js 有一个内置 [`child_process`](https://nodejs.cn/api/child_process.html) 模块。它提供了一些方法，允许我们编写在子进程中运行命令的脚本。

这些命令可以运行安装在我们运行脚本的机器上的任何程序。

## 什么是 execa？

[`execa`](https://www.npmjs.com/package/execa) 为 `child_process` 模块提供了一个包装器，以便于使用。

本文将介绍 `execa` 的好处，以及如何使用它。

## 使用 `execa` 的好处

`execa` 相对于内置的 Node.js `child_process` 模块有几个好处。

首先，`execa` 公开了一个基于 promise 的 API。这意味着我们可以在代码中使用 `async/await`，而不需要像使用异步 `child_process` 模块方法那样创建回调函数。如果我们需要它，还有一个 `execaSync` 方法可以同步运行命令。

`execa` 还可以方便地转义并引用我们传递给它的任何命令参数。例如，如果我们传递一个带有空格或引号的字符串，`execa` 将为我们处理转义。

程序在输出的末尾添加一行新行是很常见的。这有利于命令行的可读性，但在脚本上下文中没有帮助，因此默认情况下，`execa` 会自动为我们删除这些新行。

如果执行脚本（`node`）的进程因任何原因死掉，我们可能不希望子进程挂起。`execa` 会自动为我们清理子进程，确保我们不会出现“僵尸”进程。

使用 `execa` 的另一个好处是，它支持在 Windows 上使用 Node.js 运行子进程。它使用了流行的 [`cross-spawn`](https://www.npmjs.com/package/cross-spawn) 包，该包可以解决 Node.js 的已知问题。

## 开始使用 `execa`

创建并切换到 `tutorial` 目录：

```js
mkdir execa-test
cd execa-test
```

初始化项目：

```js
npm init -y
```

安装 `execa`

```js
npm i execa
```

### 在 Node.js 中使用纯 ES 模块包

Node.js 支持两种不同的模块类型：

- [CommonJS 模块](https://nodejs.cn/api/modules.html)（CJS）：使用 `module.exports` 以导出函数和对象，并 `require()` 将其加载到另一个模块中
- [ECMAScript 模块](https://nodejs.cn/api/esm.html)（ESM）：使用 `export` 导出函数和对象，并使用 `import` 将它们加载到另一个模块中

[`execa` 在其 v6.0.0 版本](https://github.com/sindresorhus/execa/releases/tag/v6.0.0)成为一个纯 ESM 包。这意味着我们必须使用支持 ES 模块的 Node.js 版本才能使用这个包。

对于我们自己的代码，我们可以显示 Node.js 通过在 `package.json` 中添加 `"type": "module"`，确保我们项目中的所有模块都是 ES 模块。或者我们可以将单个脚本的文件扩展名设置为 `.mjs`。

打开 `package.json` 文件，添加：

```json
{
  "type": "module"
}
```

现在，我们可以在创建的任何脚本中从 `execa` 包导入 `execa` 方法：

```js
import { execa } from 'execa'
```

尽管 ES 模块在 Node.js 包和应用程序中得到采用，但 CommonJS 模块仍然是使用最广泛的模块类型。如果由于任何原因您无法在代码库中使用 ES 模块，则需要 `import` 在 CommonJS 模块中使用该方法：

```js
async function run() {
  const { execa } = await import('execa')

  // ...
}

run()
```

**注意，**我们需要将对 `import` 函数的调用包装在一个 `async` 函数中，因为 CommonJS 模块不支持[顶级 `await`](https://nodejs.cn/api/esm.html#top-level-await)。

> 详细内容可以阅读阮一峰老师的 [Node.js 如何处理 ES6 模块](https://www.ruanyifeng.com/blog/2020/08/how-nodejs-use-es6-module.html)。

### 使用 `execa` 运行命令

现在，我们将创建一个脚本，使用 `execa` 运行以下命令：

```js
echo "Process execution for humans"
```

`echo` 程序打印出传递给它的文本字符串。

创建 `run.js` 文件，编写如下内容：

```js
import { execa } from 'execa'

// execa 返回的 promise 与对象解析。
const { stdout } = await execa('echo', ['Process execution for humans'])
console.log({ stdout })
```

运行文件：

```js
$ node run.js
{ stdout: '"Process execution for humans"' }
```

### `stdout` 是什么？

程序可以访问“标准流”。本文使用了其中两个：

- `stdout` — 标准输出是程序将其输出数据写入的流
- `stderr` — 标准错误是程序写入错误消息并调试数据的流

当我们运行程序时，这些标准流通常连接到父进程。如果我们在终端上运行一个程序，这意味着终端将接收并显示程序发送到 `stdout` 和 `stderr` 流的数据。

当我们运行本文中的脚本时，子进程的 `stdout` 和 `stderr` 流连接到父进程 node，允许我们访问子进程发送给它们的任何数据。

### 捕获 `stderr`

调用 `execa` 方法从对象解构 `stderr` 属性：

```js
import { execa } from 'execa'

const { stdout, stderr } = await execa('echo', ['Process execution for humans'])
console.log({ stdout, stderr }) // { stdout: 'Process execution for humans', stderr: '' }
```

再次运行后，您会发现 `stderr` 的值是一个空字符串（`''`）。这是因为 `echo` 命令没有向标准错误流写入任何数据。

`ls(1)` 程序列出有关文件和目录的信息。如果文件不存在，它将向标准错误流写入错误消息。

替换代码：

```js
const { stdout, stderr } = await execa('ls', ['file.txt'])
```

运行后，将输出以下内容：

```js
Error: Command failed with exit code 1: ls file.txt
...
{
  shortMessage: 'Command failed with exit code 2: ls file.txt',
  command: 'ls file.txt',
  escapedCommand: 'ls file.txt',
  exitCode: 1,
  // ...
}
```

使用 `execa` 方法运行此命令时引发错误。这是因为子进程的退出代码不是 0。退出代码为 0 通常表示进程已成功运行，而任何其他值都表示存在问题。

`execa` 方法抛出的错误对象包含一个 `stderr` 属性，其中包含由 `ls` 写入标准错误流的数据。

我们现在需要实现错误处理，这样如果命令失败，我们就可以优雅地处理它，而不是让它破坏我们的脚本。

> **注意**：程序通常会成功运行，但也会将调试消息写入 `stderr`。

### `execa` 中的错误处理

我们可以试着把命令包装 `try...catch` 语句并输出自定义错误消息，如下所示：

```js
try {
  const { stdout, stderr } = await execa('ls', ['file.txt'])

  console.log({ stdout, stderr })
} catch (error) {
  console.error(
    `ERROR: The command failed. stderr: ${error.stderr} (${error.exitCode})`
  )
}
```

运行脚本后，输出：

```txt
ERROR: The command failed. stderr: ls: cannot access 'file.txt': No such file or directory (1)
```

### 取消子进程

一旦我们开始执行一个命令，我们可能想要取消这个过程，例如，如果它需要比预期更长的时间才能完成。`execa` 提供了一个 `cancel` 方法，我们可以调用它向子进程发送 `SIGTERM` 信号。

```js
// 创建一个仅运行 5s 的子进程
// 注意：这里不需要加 await，这里演示 1s 后取消子进程
const subprocess = execa('sleep', ['5s'])

// 1s 后取消子进程
setTimeout(() => {
  subprocess.cancel()
}, 1000)

try {
  const { stdout, stderr } = await subprocess

  console.log({ stdout, stderr })
} catch (error) {
  if (error.isCanceled) {
    console.error(`ERROR: The command took too long to run.`)
  } else {
    console.error(error)
  }
}
```

当 `subprocess.cancel` 方法被调用时，错误对象上的 `isCanceled` 属性被设置为`true`。这使我们能够确定错误的原因并显示特定的错误消息。

运行 `node run.js`，输出：

```txt
ERROR: The command took too long to run.
```

如果我们需要强制一个子进程结束，我们可以调用 `subprocess.kill` 方法而不是 `subprocess.cancel`。

### 使用 `execa` 从子进程管道输出

`execa` 方法返回的对象中的 `stdout` 和 `stderr` 属性是可读的流。我们已经看到了如何将发送到这些流的数据保存在变量中。例如，我们可以通过管道将可读流转换为可写流，以便在子进程运行时查看其输出。

去除定时器，更改子进程的代码，然后，我们将通过管道将 `stdout` 流从子进程传输到父进程的 `stdout` 流：

```js
const subprocess = execa('echo', ["is it me you're looking for?"])

subprocess.stdout.pipe(process.stdout)
```

```txt
is it me you're looking for?
{ stdout: "is it me you're looking for?", stderr: '' }
```

### 将输出重定向到文件

我们可以将子进程的 `stdout` 数据写入一个文件，而不是通过管道传输到父进程的 `stdout` 流。

首先，导入内置 `fs` 模块：

```js
import fs from 'fs'
```

然后，我们可以替换对 `pipe` 方法的现有调用：

```js
subprocess.stdout.pipe(fs.createWriteStream('stdout.txt'))
```

这将创建一个 `fs.WriteStream` 实例，其中包含来自子流程的数据。`stdout` 可读流将通过管道传输到。

运行脚本，输出：

```txt
{ stdout: "is it me you're looking for?", stderr: '' }
```

我们还应该看到一个名为 `stdout.txt` 的文件，其中包含来自子进程标准输出流的数据：

```bash
$ cat stdout.txt
is it me you're looking for?
```
