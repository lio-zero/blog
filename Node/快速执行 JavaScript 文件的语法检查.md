# 快速执行 JavaScript 文件的语法检查

Node.js 提供了一种检查 JavaScript 文件是否语法有效的方法。

## 从命令行检查 JavaScript 语法

Node.js `--check` 选项在运行 JavaScript 文件时可用。

以 `var a =` 为例，执行命令将输出：

```bash
$ node --check test.js

SyntaxError: Unexpected end of input
    at Object.compileFunction (node:vm:352:18)
    at wrapSafe (node:internal/modules/cjs/loader:1033:15)
    at checkSyntax (node:internal/main/check_syntax:66:3)
```

`--check` 标志将关闭 [Node.js](https://nodejs.org/en/) 二进制文件转换为 JavaScript 语法检查器，用于解析源代码并查找无效语法。Node.js 没有在此**检查模式**下运行任何代码。

> Node Docs：`--check` 在不执行脚本的情况下检查脚本的语法。如果脚本无效，则退出并显示错误代码。

如果您正在转换代码，并且希望确保代码转换生成有效的 JavaScript，那么这样的快速语法检查将非常方便。

## vm 模块

在研究 `---check` 选项时，我还了解了 [vm 模块](https://nodejs.org/api/vm.html)。`vm` 模块是 Node.js 核心的一部分。

您可以使用它从脚本中评估和语法检查 JavaScript 文件。

```js
const vm = require('vm')
const script = new vm.Script('var a =')
```

如果在提供的 JavaScript 代码字符串中存在任何语法错误，`vm.Script` 的构造函数将抛出异常。

`--check` 和 `vm` 模块看起来很有趣。因此，如果您正在生成或转换代码，请使用它们包含并构建自己的 JavaScript 语法检查器。
