# Tree Shaking

## 浏览器原生 ES 模块和性能问题

ES 模块为您提供了一种原生方式，可以将代码分解为更小的模块化部分，并将变量和函数的范围限制在需要的地方。

当您 `import` 导入函数或变量时，必须下载该模块的整个文件。如果只从一个包含数百个函数的文件中导入一个函数，那么最终下载的 JavaScript 将远远超过实际需要的。

例如，假设我们有一个导出三个实用函数的工具库。

```js
// utils.js
export function shuffle() {}
export function foo() {}
export function baz() {}

export { shuffle, foo, baz }
```

在另一个文件中，我们要使用 `shuffle()` 函数，所以我们使用 `import` 导入它。

```js
import { shuffle } from './path/to/utils.js'

let name = ['O.O', 'D.O', 'K.O', 'O.K']
shuffle(name)
```

如果您使用浏览器原生 ES 模块，使用 `[type="module"]` 加载脚本，则整个 `utils.js` 文件将由浏览器下载、编译和解析。

您只需要 `shuffle()`，但浏览器必须抓取整个文件才能为您获取该函数。可以想象，这是一个性能问题，尤其是对于较大的库。

这就是需要 **Tree Shaking** 的原因。

## 什么是 Tree Shaking，它是如何工作的？

模块打包器是获取 JavaScript 文件并将所有 `import` 函数和变量组合或连接到单个文件中的工具。

为了尽可能提高文件的性能，现代的模块打包器，比如 [webpack](https://webpack.docschina.org/)、[rollup.js](https://www.rollupjs.org/) 或 [esbuild](https://esbuild.github.io/) 使用一个名为 **tree shaking** 的过程。

> `Tree Shaking` 指基于 ES Module 进行静态分析，通过 AST 将用不到的函数进行移除，从而减小打包体积。

当他们在文件中遇到 `import` 时，他们只会导入您指定的函数和变量（以及他们使用的任何内部变量或函数）。使用上面的示例，捆绑文件将仅包含 `shuffle()` 函数和 `name` 数组。

```js
function shuffle(arr) {
  // do something...
}

let name = ['Gandalf', 'Radagast', 'Merlin']
shuffle(name)
```

浏览器原生 ES 模块还有其他性能挑战，因此如果您正在使用它们，我建议您使用模块打包器。

## webpack 的 Tree Shaking

从 webpack 4+ 开始，生产环境下 webpack 会自动为您优化和减少捆绑包的大小，它使用的优化技术是 [Tree Shaking](https://webpack.js.org/guides/tree-shaking/)。本质上，它是一种用于删除未使用代码的优化技术，通过分析静态的 ES 模块，来移除未使用的代码。

您还可以通过在配置文件中添加带有特定字段的 [`optimization` 对象](https://webpack.js.org/configuration/optimization/)来手动优化应用程序。

`minimize` 字段用于指示 webpack 最小化包大小。默认情况下，webpack 将尝试使用 [TerserPlugin](https://www.npmjs.com/package/terser-webpack-plugin) 来实现这一点，它是 webpack 附带的一个代码压缩包。

> `minimize` 适用于通过从代码中删除不必要的数据来最小化代码，从而减少处理后生成的代码大小。

我们还可以通过在 `optimization` 对象中添加一个 `minimizer` 数组字段来使用其他首选缩小器。下面的 [uglifyjs-webpack-plugin](https://www.npmjs.com/package/uglifyjs-webpack-plugin) 就是一个例子。

```js
// webpack.config.js
const Uglify = require('uglifyjs-webpack-plugin')

module.exports = {
  optimization {
    minimize : true,
    minimizer : [
      new Uglify({
        cache : true,
        test: /\.js(\?.*)?$/i,
      })
    ]
  }
}
```

`uglifyjs-webpack-plugin` 有两个非常重要的选项。首先，启用 `cache` 意味着只有当现有文件发生新更改时，Uglify 才会缩小它们，而 `test` 选项指定了我们要缩小的特定文件类型。

> **注意**：[uglifyjs-webpack-plugin](https://www.npmjs.com/package/uglifyjs-webpack-plugin) 提供了一个全面的选项列表，可用于缩小代码。

## 引入支持 Tree Shaking 的 Package

为了减小生产环境体积，我们可以**使用一些支持 ES 的 package，比如使用 `lodash-es` 替代 `lodash`**。

我们可以在 [npm.devtool](https://npm.devtool.tech/lodash-es)中查看某个库是否支持 Tree Shaking。

查看两者对比：

![lodash-es](https://upload-images.jianshu.io/upload_images/18281896-8539dd5cf141adf7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![lodash](https://upload-images.jianshu.io/upload_images/18281896-62582741c8395bdc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
