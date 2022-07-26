# JavaScript 的 AST 抽象语法树

[**抽象语法树**](https://zh.wikipedia.org/wiki/%E6%8A%BD%E8%B1%A1%E8%AA%9E%E6%B3%95%E6%A8%B9)（Abstract Syntax Tree，AST）是程序源代码的树形表示。

![所有这些库，无论以何种方式，都建立在 AST 处理之上](https://upload-images.jianshu.io/upload_images/18281896-ddd5073aa76cf2da.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

有几个 JavaScript AST 标准：

- [estree](https://github.com/estree/estree) — ECMAScript AST 的标准；
- [shift](https://shift-ast.org/) — 设计时考虑了转换，[与 estree 不兼容](https://blog.shapesecurity.com/2015/01/06/a-technical-comparison-of-the-shift-and-spidermonkey-ast-formats/)；
- [babel](https://github.com/babel/babel/blob/main/packages/babel-parser/ast/spec.md) — 支持尚未成为标准但有[提案](https://github.com/tc39/proposals)的语言功能。

以下是一部分 JavaScript 解析器列表：

- [esprima](https://esprima.org/) 是一个高性能、符合标准的 ECMAScript 解析器。
- [acorn](https://github.com/acornjs/acorn) 非常受欢迎，快速且稳定
- [espree](https://github.com/eslint/espree) 基于 `acorn`，用于 [eslint](https://eslint.org/) 中；
- [@babel/parser](https://babeljs.io/docs/en/babel-parser) 基于 `acorn`，支持所有新的语言特性
- [shift-parser](https://shift-ast.org/parser.html) 生成 Shift 格式 AST 的 ECMAScript 解析器
- [typescript](https://github.com/Microsoft/TypeScript/wiki/Using-the-Compiler-API) 可以解析 JavaScript 和 TypeScript，为此生成它自己的 AST 格式

您可以在 [AST explorer](https://astexplorer.net/) 找到更多解析器，其中大多数是 `estree` 兼容的。

> AST explorer 是一个在线工具，用于探索由 10 多个解析器生成的 AST。它是学习一门语言的 AST 树的好工具。

虽然大多数支持 `estree` 的解析器可以很容易地相互替换，但 `babel` 有更广泛的基础设施，可以轻松地使用 AST。它有：

- [handbook](https://github.com/jamiebuilds/babel-handbook/blob/master/translations/en/plugin-handbook.md) 描述所有工具及其使用方法。
- [@babel/traverse](https://babeljs.io/docs/en/babel-traverse.html) — 维护整个树状态，并负责替换、删除和添加节点；
- [@babel/template](https://babeljs.io/docs/en/babel-template) — 从字符串创建 AST 节点的最简单方法。
- [@babel/types](https://babeljs.io/docs/en/babel-types) — 包含 AST 节点的生成器和检查器。

使用 AST 最简单的方法之一是使用 [putout](https://github.com/coderaiser/putout)，它基于 `babel` 并支持借助 [plugins API](https://github.com/coderaiser/putout#plugins-api) 转换 JavaScript 代码的简化方法。

以下是删除 `DebuggerStatement` 节点的示例：

```js
module.exports.replace = () => ({
  debugger: ''
})
```

如果要转换变量的位置，请更改声明方式：

```js
module.exports.replace = () => ({
  'let __a = __b': 'const __b = __a'
})
```

如果您想将以下代码转换为 `return x[0]`：

```js
for (const x of y) {
  return x
}
```

您可以使用：

```js
module.exports.replace = () => ({
  'for (const __a of __b) {return __a}': 'return __a[0]'
})
```

借助 `putout`，您可以对 JavaScript 代码进行最简单的转换，而无需直接使用 AST 进行处理。

推荐一篇面向 JS 的 AST 介绍 👉 [AST for JavaScript developers](https://itnext.io/ast-for-javascript-developers-3e79aeb08343)

## 更多资料

- [How to Modify Nodes in an Abstract Syntax Tree](https://css-tricks.com/how-to-modify-nodes-in-an-abstract-syntax-tree/)
- [JAVASCRIPT AST VISUALIZER](https://resources.jointjs.com/demos/javascript-ast) — 一个 JavaScript AST 可视化工具
