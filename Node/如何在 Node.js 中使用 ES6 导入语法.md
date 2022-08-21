# 如何在 Node.js 中使用 ES6 导入语法

> **模块**是导出一个或多个值的 JavaScript 文件。导出的值可以是变量、对象或函数。

Node.js 应用由模块组成，其[模块系统](http://nodejs.org/docs/latest/api/modules.html)采用 CommonJS 规范，它并不是 JavaScript 语言规范的正式组成部分。

在 CommonJS 中，有一个全局方法 `require()`，用于加载模块。

```js
// 加载 path 模块
const path = require('path')
```

而 ECMAScript 模块（简称 ES 模块或 ESM）是 JavaScript 语言规范中添加的一个模块，正在寻求统一和标准化模块在 JavaScript 应用程序中的加载方式。

以下导入语法由以下 ES 模块标准组成，用于导入从不同 JavaScript 文件导出的模块：

```js
import XXX from 'xxx'
```

Node.js 不支持直接导入 ES6。尝试在 JS 文件中编写 `import` 语法：

```js
// index.js
import { sep } from 'path'

console.log('print: ', sep)
```

使用 `node index.js` 运行 Node.js 服务器，您将遇到以下错误：

![error](https://upload-images.jianshu.io/upload_images/18281896-f0fd1dce05e2d705.image?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

而目前最快速的解决方法是，我们可以使用 [Node.js 推荐的方法](https://nodejs.org/api/esm.html#esm_enabling)，在 `package.json` 文件中设置 `"type": "module"`。

```json
{
  "type": "module"
}
```

此解决方案适用于最新的 Node.js 版本 `14.x.x` 以上的版本（撰写本文时为 `15.6.0`）。

![success](https://upload-images.jianshu.io/upload_images/18281896-04fd8022e7a4d7a6.image?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 低于 Node v.14 版本的环境

另一个解决这个问题的方法是使用 [**Babel**](https://babeljs.io/)。它是一个 JavaScript 编译器，允许您使用最新语法编写 JS。它可以在任何用 JavaScript 编写的项目中使用，因此也可以在 Node.js 项目中使用。

首先从终端窗口安装以下依赖项：

```bash
npm i -D @babel/core @babel/preset-env @babel/node
```

它们后面将用于转化 ES6 以上语法。

然后在 Node.js 项目的根目录下创建一个名为 `babel.config.json` 的文件，并添加以下内容：

```json
module.exports = {
  "presets": ["@babel/preset-env"]
}
```

`@babel/node` 包是一个 CLI 实用程序，它在运行 Node.js 项目之前用 Babel 预设和插件编译 JS 代码。这意味着它将在执行 Node 项目之前读取并应用 `babel.config.json` 中提供的任何配置。

运行命令执行脚本时，使用 `babel-node` 替换 `node`。

为了方便使用，我们在 `package.json` 中配置一个 npm script。例如，使用 `npm run dev` 脚本运行 Node 服务器的示例：

```json
{
  "scripts": {
    "dev": "node --exec babel-node index.js"
  }
}
```

您还可以不使用上面提到的方法，以最简单的方式使用 ES 模块。在创建时，以 `.cjs` 和 `.mjs` 扩展区分使用 CommonJS 还是 ES 模块。

## 更多资料

- [Babel 入门](https://github.com/lio-zero/blog/blob/main/%E5%89%8D%E7%AB%AF/Babel%20%E5%85%A5%E9%97%A8.md)
- [Node.js 如何处理 ES6 模块](https://www.ruanyifeng.com/blog/2020/08/how-nodejs-use-es6-module.html)
- [How to Use ECMAScript Modules in Node.js](https://dmitripavlutin.com/ecmascript-modules-nodejs/)
