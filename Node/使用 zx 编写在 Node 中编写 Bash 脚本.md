# 使用 zx 编写在 Node 中编写 Bash 脚本

Bash 是一种命令语言，通常作为命令行解释程序出现，用户可以在其中从他们的终端软件执行命令。例如，我们可以使用 Ubuntu 的终端来运行 Bash 命令。我们还可以通过 shell 脚本创建和运行 Bash 脚本文件。

我们经常在许多自动化场景中使用 shell 脚本，例如构建过程、CI/CD 或与计算机维护相关的活动。作为一种功能齐全的命令语言，Bash 支持管道、变量、函数、控制语句和基本算术运算。

然而，Bash 不是一种通用的开发人员友好型编程语言。它不支持 OOP、JSON 等结构、数组以外的通用数据结构，以及内置的字符串或数组操作方法。这意味着程序员通常需要从 Bash 中调用单独的 Python 或 Node 脚本来满足这些需求。

这就是 [zx](https://github.com/google/zx) 项目的用武之地。zx 引入了一种使用 JavaScript 编写类似 Bash 的脚本的方法。

相比之下，JavaScript 几乎具有开发人员所需的所有内置功能。zx 允许我们通过为几个关键的 CLI 相关 Node.js 包提供包装 API（如：`child_process`），用 JavaScript 编写 shell 脚本。因此，可以使用 zx 编写开发人员友好的、类似 Bash 的 shell 脚本。

本文将介绍 zx 的一些用法，如何在项目中使用它。

## zx 是如何工作的？

每个 zx shell 脚本文件都有 `.mjs` 扩展名。第三方 API 的所有内置函数和包装器都是预先导入的。因此，您不必在基于 JavaScript 的 shell 脚本中使用额外的 `import` 语句。

zx 接受来自标准输入、文件和 URL 的脚本。它将您的 zx 命令集导入为 ECMAScript 模块（MJS）来执行，命令执行过程使用 [Node.js 的 `child_process` API](https://nodejs.org/api/child_process.html)。

## 安装

全局安装 zx：

```js
npm i -g zx
```

版本要求：`node >= 16.0.0`

在终端运行 zx 以检查程序是否已成功安装：

```bash
zx

Usage:
   zx [options] <script>

 Options:
   --quiet            : don't echo commands
   --shell=<path>     : custom shell binary
   --prefix=<command> : prefix all commands
   --experimental     : enable new api proposals
   --version, -v      : print current zx version
```

你也可以单独在项目引入。

## 顶层 `await`

为了在 Node.js 中使用顶级 `await`，在异步函数之外进行 `await`，我们需要在 ES 模块中编写代码，它支持顶级 `await`。

我们可以通过在 `package.json` 中添加 `"type": "module"` 来表示项目中的所有模块都是 ES 模块，或者我们可以将单个脚本的文件扩展名设置为 `.mjs`。

## 基本语法

首先，文件扩展使用 `.mjs` 以便可以在顶层使用 `await`，如果您还是喜欢原来的 `.js`，需要包装一个 `async` 函数。

其次，需要在文件的顶部加上 `#!/usr/bin/env zx`。

以下是获取项目的当前 Git 分支示例：

```js
// demo.mjs
#!/usr/bin/env zx

const branch = await $`git branch --show-current`
console.log(`当前分支：${branch}`)
```

- `#!/usr/bin/env zx` 告诉操作系统的脚本执行器选择正确的解释器。
- `$` 是一个函数，它执行给定的命令，并在与 `await` 关键字一起使用时返回其输出。

最后，我们使用 `console.log` 来显示当前分支。

运行脚本，以获取项目的当前 Git 分支：

```bash
zx demo.mjs
```

它还将显示您执行的每个命令，因为 zx 在默认情况下打开了详细模式。

通过添加以下内容，即可消除额外的命令详细信息。

```js
$.verbose = false
```

由于我们在第一行使用了 [shebang](https://en.wikipedia.org/wiki/Shebang_%28Unix%29)（`#!`），我们也可以在不使用 zx 命令的情况下运行脚本。

```bash
chmod +x ./demo.mjs
```

## 颜色和格式

zx 暴露了 chalk API。因此，我们可以使用它进行着色和格式化，如下所示。

```js
#!/usr/bin/env zx

$.verbose = false
let branch = 1
chalk.level = 1
console.log(`当前分支：${chalk.red.bold(branch)}`)
```

查看 [chalk 的官方文档](https://www.npmjs.com/package/chalk)以了解更多的着色和格式化方法。

## 用户输入和命令行参数

zx 提供提问功能，从命令行界面捕获用户输入。您还可以使用选项来启用传统的 UNIX 选项卡完成功能。

它使用的是 Node 的 [readline](https://nodejs.org/api/readline.html) 包。

以下脚本将捕获文件名和模板。它使用用户输入的配置构建一个文件。

```js
#!/usr/bin/env zx
$.verbose = false
let filename = await question('文件名是什么? ')
let template = await question('你最喜欢的模板是什么? ', {
  choices: ['function', 'class']
})
let content = ''

if (template == 'function') {
  content = `function main() {
    console.log('Test')
}`
} else if (template == 'class') {
  content = `class Main {
    constructor() {
        console.log('Test')
    }
}`
} else {
  console.error(`无效模板: ${template}`)
  process.exit()
}

fs.outputFileSync(filename, content)
```

已解析的命令行参数对象可作为全局 `argv` 常量使用。解析是使用 [minimist](https://www.npmjs.com/package/minimist) 模块完成的。

下面的示例捕获了两个命令行参数值。

```js
#!/usr/bin/env zx
$.verbose = false
const size = argv.size
const isFullScreen = argv.fullscreen
console.log(`size=${size}`)
console.log(`fullscreen=${isFullScreen}`)
```

运行上述脚本文件，以检查命令行参数的支持。

```bash
$ zx ./demo .mjs --size=1080 --fullscreen

# size=1080
# fullscreen=true
```

## 网络请求

我们经常使用 curl 命令通过 Bash 脚本发出 HTTP 请求。zx 为 [node-fetch](https://www.npmjs.com/package/node-fetch) 模块提供了一个包装器，它将特定模块的 API 公开为 fetch。优点是 zx 不会像 Bash 使用 curl 那样为每个网络请求生成多个进程  ，因为 node-fetch 包使用 Node 的标准 HTTP API 来发送网络请求。

以下使用 zx 发送 HTTP 请求：

```js
#!/usr/bin/env zx

$.verbose = false
let response = await fetch('https://jsonplaceholder.typicode.com/todos/1')
if (response.ok) console.log(await response.text())

/*
{
  "userId": 1,
  "id": 1,
  "title": "delectus aut autem",
  "completed": false
}
*/
```

## 构建命令管道

在 shell 脚本中，管道指的是多个顺序执行的命令。我们经常在 shell 脚本中使用众所周知的管道字符（`|`），将输出从一个进程传递到另一个进程。zx 提供了两种不同的方法来构建管道。

我们可以将 `|` 字符与类似于 Bash 脚本的命令集一起使用  ，或者我们也可以使用 zx 内置 API 中的 `pipe()` 方法。

使用 `|`：

```js
#!/usr/bin/env zx

$.verbose = false
let greeting = await $`echo "hello World" | tr '[h]' [H]`
console.log(`${greeting}`)
```

使用 `pipe()`：

```js
#!/usr/bin/env zx

$.verbose = false
let greeting = await $`echo "Hello world"`.pipe($`tr '[w]' [W]`)
console.log(`${greeting}`)
```

## 高级用法

除了基于 JavaScript 的 shell 脚本支持外，zx 还支持其他一些有用的功能。

默认情况下，zx 使用 Bash 解释器来运行命令。我们可以通过修改 `$.shell` 来更改默认 shell 配置变量。下面的脚本使用 `sh` shell 而不是 bash。

```js
$.shell = '/usr/bin/sh'
$.prefix = 'set -e;'

$`echo "Your shell is $0"` // Your shell is /usr/bin/sh
```

zx 命令行程序也可以从 URL 运行远程脚本。以提供文件名的方式提供 zx 脚本的链接。

以下是 zx 仓库提供的一个示例：

```js
zx https://github.com/google/zx/blob/main/examples/interactive.mjs
```

其他：

- 与 TypeScript 一起使用
- GitHub Actions
- etc

详细内容可以查看[官方文档](https://github.com/google/zx#readme)。
