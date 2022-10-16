# Node REPL

REPL（Read Eval Print Loop，交互式解释器）是一个处理 Node.js 表达式的交互式 shell，类似 Window 系统的终端或 UNIX/Linux shell，我们可以在终端中输入命令，并接收系统的响应。

REPL 与 Node.js 安装捆绑在一起，并执行以下所需的任务：

- Read − 读取用户输入的 JavaScript 代码
- Eval − 解释代码行的结果
- Print− 将结果打印给用户
- Loop − 循环执行上述操作，直到用户发出退出信号（按下两次 Ctrl + C）。

## 基本用法

安装完 Node，您就拥有了 REPL，在命令行输入 `node` 进入 REPL：

```bash
node
```

您可以计算一些简单的表达式：

```bash
> 'Hello' + ' Node'
'Hello Node'
> console.log('Hi')
Hi
> 10 * 2
20
> var x = 10
undefined
> var y = 20
undefined
> x + y
30
```

下面重点介绍一下，它的几个重要的命令。

## REPL 命令

它们都以点（`.`）开头，其中一个便是 `.help`，我们在命令行中输入它，以查看其它可用命令：

- `.break` — 用于中断正在执行的操作
- `.clear` — `.break` 别名
- `.editor` — 进入编辑模式
- `.exit` — 退出 REPL
- `.help` — 列出所有可用的命令
- `.save` — 将 JS 从文件加载到 REPL 会话中
- `.load` — 将 REPL 会话中的所有求值命令保存到一个文件中

这里不提供示例，您可以尝试一下它们，都是一些简单的命令。

关于 REPL 的一些用法请查阅官网 [repl 章节](https://nodejs.org/api/repl.html)或 [How To Use the Node.js REPL](https://www.digitalocean.com/community/tutorials/how-to-use-the-node-js-repl)。

## REPL 中下划线变量

使用 `_` 获取最后一个结果。

```bash
> var x = 10
undefined
> var y = 20
undefined
> x + y
30
> _
30
```

## 创建自定义 Node.js REPL

Node 还提供了一个 [REPL 模块](https://nodejs.org/api/repl.html) 来实现与内置 REPL 同样的效果。

您只需几行 JavaScript 代码就可以创建自定义 REPL：

```js
const repl = require('repl')

// 定义可用的方法和状态
const state = {
  print() {
    console.log("That's awesome!")
  }
}

const myRepl = repl.start("Welcome to lio-zero repl > ")

Object.assign(myRepl.context, state)
```

## 最后

它很方便，我们可以使用它来计算一些简单的表达式，但我相信，您基本上不会用到 REPL 吧！😂😂
