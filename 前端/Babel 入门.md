# Babel 入门

Babel 是一个很棒的工具，它已经存在了相当长的一段时间，但是现在几乎每个 JavaScript 开发人员都依赖它，而且这种情况还会继续，因为 Babel 现在是不可或缺的，并为每个 Web 开发人员解决了一个大问题。

**哪个问题？**

每个 Web 开发人员都有一个问题：JavaScript 的一项功能在最新版本的浏览器中可用，但在旧版本中不可用。或者 Chrome 或 Firefox 实现了它，但 IE 和 Edge 没有。

例如，箭头函数：

```js
;[1, 2, 3].map((n) => n + 1)
```

现在所有现代浏览器都支持它。IE11 不支持它，Opera Mini 也不支持，您可以通过以下方式查看 JS 语法的兼容情况：

- [Can I Use](https://caniuse.com/)
- [ES6 Compatibility Table](https://kangax.github.io/compat-table/es6/)

**那么你应该如何处理这个问题呢？**你应该继续前进，把那些浏览器比较旧/不兼容的客户抛在脑后，还是应该编写更旧的 JavaScript 代码来让所有用户都满意？

Babel 是一个编译器：它接收以一种标准编写的代码，并将其转译成另一种标准的代码。

您可以配置 Babel 以将现代 ES6+ 语法转换为 ES5 语法：

```js
;[1, 2, 3].map(function (n) {
  return n + 1
})
```

这必须在构建时发生，因此您必须设置一个工作流来为您处理此问题。Webpack 是一种常见的解决方案，但现在，我更多的使用 Vite。

## 安装 Babel

在本地安装 Babel：

```bash
npm i -D @babel/core @babel/cli
```

在过去，很多人都会建议全局安装 `@babel/cli`，但现在 babel 维护人员不鼓励这样做，因为通过在本地使用它，您可以在每个项目中拥有不同版本的 babel，并且在您的存储库中签入 babel 更有利于团队工作。

使用 npm 自带的 npx 运行 babel：

```bash
npx babel script.js
```

## Babel 示例配置

Babel 开箱即用并没有做任何有用的事情，您需要对其进行配置并添加插件。

> [Babel 插件列表](https://babeljs.io/docs/en/plugins)

为了解决介绍中提到的问题（在每个浏览器中使用箭头函数），我们安装：

```bash
npm i -D @babel/plugin-transform-arrow-functions
```

在 `.babelrc` 配置：

```json
{
  "plugins": ["@babel/plugin-transform-arrow-functions"]
}
```

以下是一个示例：

```js
// script.js
var a = () => {}
var a = (b) => b

const double = [1, 2, 3].map((num) => num * 2)
console.log(double) // [2,4,6]

var bob = {
  _name: 'Bob',
  _friends: ['Sally', 'Tom'],
  printFriends() {
    this._friends.forEach((f) => console.log(this._name + ' knows ' + f))
  }
}
console.log(bob.printFriends())
```

运行 `npx babel script.js`，将输出：

```js
var a = () => {}
var a = (b) => b

const double = [1, 2, 3].map((num) => num * 2)
console.log(double) // [2,4,6]

var bob = {
  _name: 'Bob',
  _friends: ['Sally', 'Tom'],
  printFriends() {
    this._friends.forEach((f) => console.log(this._name + ' knows ' + f))
  }
}
console.log(bob.printFriends())
```

## Babel 预设

上面，我们介绍了如何将 Babel 配置为编译为特定的 JavaScript 特性。

你可以添加更多的插件，但你不能一个一个地添加到配置特性，这是不现实的。

这就是 Babel 提供预设的原因。

官方预设有：

- [`@babel/preset-env`](https://babeljs.io/docs/en/babel-preset-env) 用于编译 ES2015+ 语法
- [`@babel/preset-typescript`](https://babeljs.io/docs/en/babel-preset-typescript) 用于 TypeScript
- [`@babel/preset-react`](https://babeljs.io/docs/en/babel-preset-react) 用于 React
- [`@babel/preset-flow`](https://babeljs.io/docs/en/babel-preset-flow) 用于 Flow

我们这里介绍以下 `@babel/preset-env`。

### @babel/preset-env 预设

`@babel/preset-env` 预设支持所有现代 JavaScript 特性。

默认情况下 `@babel/preset-env` 将使用 [browserslist](https://github.com/ai/browserslist#queries) 配置源，除非设置了 [`targets`](https://babeljs.io/docs/en/babel-preset-env#targets) 或 [`ignoreBrowserslistConfig`](https://babeljs.io/docs/en/babel-preset-env#ignorebrowserslistconfig) 选项。

例如，支持所有浏览器的最后 2 个版本，但对于 IE，支持自 IE 9 以来的所有版本：

```json
{
  "presets": [
    [
      "@babel-preset-env",
      {
        "targets": {
          "browsers": ["last 2 versions", "ie >= 9"]
        }
      }
    ]
  ]
}
```

或者，我不需要浏览器支持，只要让我使用 Node.js 12.0 即可：

```json
{
  "presets": [
    [
      "env",
      {
        "targets": {
          "node": "12.0"
        }
      }
    ]
  ]
}
```

[有关预设的更多信息](https://babeljs.io/docs/en/presets)

至于构建工具配置 Babel，可以查看它们各自的官网/生态，例如：webpack 需要配置 [babel-loader](https://webpack.docschina.org/loaders/babel-loader)。与配合构建工具，我们不在需要 `.babelrc` 文件。

> 这里推荐我最近新建的仓库 [frontend-focus](https://github.com/lio-zero/frontend-focus)。

另外，有一篇 Babel 7 的文章可以看看，[不容错过的 Babel7 知识](https://juejin.cn/post/6844904008679686152)，但这篇文章已经是两年前了的。前端技术栈日新月异，建议多看看官方文档。
