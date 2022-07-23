# 什么是 NPM？

[npm 是由 npm, Inc](https://www.npmjs.com/) 维护的 JavaScript 编程语言的主要包管理器。NPM 是 JavaScript 运行时环境 Node.js 的默认包管理器。它既充当注册表，也提供命令行工具。它被认为[是世界上最大的软件注册中心](https://docs.npmjs.com/about-npm/)。

npm 为前端代码（要在浏览器中运行的 JavaScript 包）和后端代码（Node.js 库）提供服务。

虽然有一些 npm 的替代品，例如 [yarn](https://yarnpkg.com/) 或者 [pnpm](https://pnpm.io/)，但它们通常只提供命令行工具，但实际上使用 npm 作为注册表或包的源。

> 安装 Node.js 时，npm 程序就安装在您的计算机上。

## 什么是包？

Node.js 中的包包含模块所需的所有文件。

模块是可以包含在项目中的 JavaScript 库。

### 下载包

下载一个包非常容易。打开命令行界面，告诉 npm 下载你想要的包。

```bash
npm i upper-case
```

下载完后，npm 会创建一个名为 `node_modules` 的文件夹，将在其中放置相关依赖包。您将来在项目上安装的所有软件包都将放置在此文件夹中。

### 使用包

像我们使用 Node 其他模块一样，我们将下载好的包，导入后使用：

```js
const uc = require('upper-case')

console.log(uc.upperCase('Hello World!'))
```

## 使用 NPM 有哪些好处？

通过 npm，您可以安装和管理项目的依赖，并且能够指明依赖项的具体版本号。

对于 Node 应用开发而言，您可以通过 `package.json` 文件来管理项目信息，配置脚本， 以及指明项目依赖的具体版本。

> 推荐 [package.json 详解](https://github.com/lio-zero/blog/blob/master/Node/package.json%20%E8%AF%A6%E8%A7%A3.md)

关于 NPM 的更多信息，你可以查阅[官方文档](https://docs.npmjs.com/cli/v6/configuring-npm/package-json)。

## 更多资料

- [awesome-npm](https://github.com/sindresorhus/awesome-npm) 收集了众多的与 npm 相关的资源。
- [how-to-npm](https://github.com/workshopper/how-to-npm)
- [Bundlephobia](https://bundlephobia.com/) 查找将 npm 包添加到您的捆绑包的成本
- [npm trends](https://www.npmtrends.com/) 相同类型 npm 包的比较
- [npmview](https://github.com/pd4d10/npmview)
- [如何对 npm package 进行发包](https://github.com/lio-zero/blog/blob/master/Node/%E5%A6%82%E4%BD%95%E5%AF%B9%20npm%20package%20%E8%BF%9B%E8%A1%8C%E5%8F%91%E5%8C%85.md)
- [防止 npm 安装不支持的 Node.js 版本](https://github.com/lio-zero/blog/blob/master/Node/%E9%98%B2%E6%AD%A2%20npm%20%E5%AE%89%E8%A3%85%E4%B8%8D%E6%94%AF%E6%8C%81%E7%9A%84%20Node.js%20%E7%89%88%E6%9C%AC.md)
- [常用的 npm 命令](https://github.com/lio-zero/blog/blob/master/Node/%E5%B8%B8%E7%94%A8%E7%9A%84%20npm%20%E5%91%BD%E4%BB%A4.md)
