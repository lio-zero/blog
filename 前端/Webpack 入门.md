# Webpack 入门

[Webpack](https://webpack.docschina.org/) 是一种用于编译 JavaScript 模块的工具，也称为模块捆绑器。

给定大量文件，它会生成一个文件（或几个文件）来运行您的应用程序。

它可以执行许多操作：

- 捆绑资源。
- 监视更改并重新运行任务。
- 可以运行 Babel 转译到 ES5，允许您使用最新的 JavaScript 功能，而不必担心浏览器支持。
- 可以将 CoffeeScript 转换为 JavaScript
- 可以将内联图像转换为 Data URI。
- 允许您对 CSS 文件使用 `require()`。
- 可以运行开发 Web 服务器。
- 可以处理热模块替换。
- 可以将输出文件拆分为多个文件，以避免在第一个页面命中时加载巨大的 js 文件。
- 可以执行 [Tree Shaking](https://github.com/lio-zero/blog/blob/main/%E5%89%8D%E7%AB%AF/Tree%20Shaking.md)（摇树）。

Webpack 不仅限于在前端使用，它在后端 Node.js 开发中也很有用。

webpack 的前身，以及仍然被广泛使用的工具，包括：

- [Grunt](https://gruntjs.com/)
- [Broccoli](https://broccoli.build/)
- [Gulp](https://gulpjs.com/)

> 推荐：[构建工具](https://github.com/lio-zero/blog/blob/main/%E5%89%8D%E7%AB%AF/%E6%9E%84%E5%BB%BA%E5%B7%A5%E5%85%B7.md)

这些和 Webpack 的功能有很多相似之处，但主要区别在于它们被称为任务运行器，而 Webpack 是作为模块捆绑器诞生的。

这是一个更专注的工具：您指定应用程序的入口点（它甚至可以是一个带有 `script` 标签的 HTML 文件），webpack 分析文件，并将运行应用程序所需的所有内容打包到一个 JavaScript 输出文件中（如果使用代码拆分，则打包到更多文件中）。

## 安装 webpack

可以为每个项目全局或本地安装 Webpack。

### 全局安装

以下是使用 Yarn 全局安装：

```bash
yarn global add webpack webpack-cli
```

使用 npm：

```bash
npm i -g webpack webpack-cli
```

完成后，您应该可以运行：

```bash
webpack-cli
```

### 本地安装

Webpack 也可以在本地安装。这是推荐的设置，因为每个项目都可以更新 webpack，而且您不太愿意只为一个小项目使用最新的功能，而不是更新所有使用 webpack 的项目。

yarn：

```bash
yarn add webpack webpack-cli -D
```

npm：

```bash
npm i webpack webpack-cli -D
```

完成后，将其添加到您的 `package.json` 文件中：

```json
{
  //...
  "scripts": {
    "build": "webpack"
  }
}
```

完成后，您可以通过键入运行以下命令来 webpack：

```bash
yarn build
```

## Webpack 配置

默认情况下，如果您遵守以下约定，webpack（从版本 4 开始）不需要任何配置：

- 应用程序的入口点是 `./src/index.js`
- 输出放在 `./dist/main.js`。
- Webpack 在生产模式下工作

> Webpack 5

当然，当您需要的时候，您可以自定义 webpack 的每一项。webpack 配置存储在项目根文件夹下的 `webpack.config.js` 文件中。

## 入口点

默认情况下，入口点为 `./src/index.js`，以下简单的示例使用 `./index.js` 文件作为入口文件：

```js
module.exports = {
  // ...
  entry: './index.js'
  // ...
}
```

## 输出

默认情况下，输出在 `./dist/main.js` 中生成。以下示例将输出包放入 `app.js` 中：

```js
module.exports = {
  // ...
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'app.js'
  }
  // ...
}
```

## Loader

使用 webpack 可以在 JavaScript 代码中使用 `import` 或 `require` 语句，不仅可以包括其他 JavaScript，还包括任何类型的文件，例如 CSS。

Webpack 旨在处理我们所有的依赖关系，而不仅仅是 JavaScript，而 loader 是实现此目的的一种方式。

例如，在您的代码中，您可以使用：

```js
import 'style.css'
```

通过使用此 loader 配置：

```js
module.exports = {
  // ...
  module: {
    rules: [{ test: /\.css$/, use: 'css-loader' }]
  }
  // ...
}
```

正则表达式以任何 CSS 文件为目标。

loader 可以有以下选项：

```js
module.exports = {
  // ...
  module: {
    rules: [
      {
        test: /\.css$/,
        use: [
          {
            loader: 'css-loader',
            options: {
              modules: true
            }
          }
        ]
      }
    ]
  }
  // ...
}
```

您可以为每个规则要求多个 loader：

```js
module.exports = {
  // ...
  module: {
    rules: [
      {
        test: /\.css$/,
        use: ['style-loader', 'css-loader']
      }
    ]
  }
  // ...
}
```

在本例中，`css-loader` 解释 `import 'style.css'` 中的 CSS 指令。然后，`style-loader` 负责使用 `<style>` 标签将 CSS 注入 DOM 中。

顺序很重要，它是相反的（最后一个先执行）。

> 这涉及到了 Loader 的执行机制。推荐：阿宝哥的[多图详解，一次性搞懂 Webpack Loader](https://juejin.cn/post/6992754161221632030)。

**有哪些 loader？**很多很多！您可以在此处找到[完整列表](https://webpack.js.org/loaders/)。

常用的 loader 是 [Babel](https://github.com/lio-zero/blog/blob/main/%E5%89%8D%E7%AB%AF/Babel%20%E5%85%A5%E9%97%A8.md)，它用于将现代 JavaScript 转换到 ES5 代码：

```js
module.exports = {
  // ...
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /(node_modules|bower_components)/,
        use: {
          loader: 'babel-loader',
          options: {
            presets: ['@babel/preset-env']
          }
        }
      }
    ]
  }
  // ...
}
```

这个示例使 Babel 预处理我们所有的 React/JSX 文件：

```js
module.exports = {
  // ...
  module: {
    rules: [
      {
        test: /\.(js|jsx)$/,
        exclude: /node_modules/,
        use: 'babel-loader'
      }
    ]
  },
  resolve: {
    extensions: ['.js', '.jsx']
  }
  // ...
}
```

[请参阅 `babel-loader` 此处的选项](https://webpack.js.org/loaders/babel-loader/)。

## 插件

插件就像 loader，但需要类固醇。它可以做 loader 做不到的事情，它是 webpack 的主要组成部分。

举个例子：

```js
module.exports = {
  // ...
  plugins: [new HTMLWebpackPlugin()]
  // ...
}
```

`HTMLWebpackPlugin` 插件的任务是自动创建 HTML 文件，添加输出 JS 包路径，这样 JavaScript 就可以提供服务了。

[有很多插件可用](https://webpack.js.org/plugins/)。

一个有用的插件，`CleanWebpackPlugin` 可以用来在创建任何输出之前清除 `dist/` 文件夹，这样在更改输出文件的名称时就不会留下文件：

```js
module.exports = {
  // ...
  plugins: [new CleanWebpackPlugin(['dist'])]
  // ...
}
```

## webpack 模式

此模式（在 webpack 4 中引入）设置了 webpack 工作的环境。它可以将其设置为 `development` 或 `production`（默认为 `production`）

```js
module.exports = {
  entry: './index.js',
  mode: 'development',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'app.js'
  }
}
```

开发模式：

- 构建速度非常快
- 优化程度低于生产模式
- 不删除注释
- 提供更详细的错误信息和建议
- 提供更好的调试体验

生产模式构建速度较慢，因为它需要生成更优化的捆绑包。生成的 JavaScript 文件的大小更小，因为它删除了生产中不需要的许多内容。

## 运行 webpack

如果全局安装，则可以从命令行手动运行 Webpack，但通常需要在 `package.json` 内编写一个脚本，然后使用 `npm` 或 `yarn` 运行。

例如，我们之前使用的 `package.json` 脚本定义：

```json
"scripts": {
  "build": "webpack"
}
```

允许我们通过运行：

```bash
npm run build
yarn run build
yarn build
```

## 观察变化

当您的应用发生变化时，Webpack 可以自动重建捆绑包，并持续监听下一个变化。

只需添加此脚本：

```json
"scripts": {
  "watch": "webpack --watch"
}
```

然后运行：

```bash
npm run watch
yarn run watch
yarn watch
```

`watch` 模式的一个不错的特性，只有在构建没有错误时才会更新捆绑包。如果有错误，`watch` 将继续监听更改，并尝试重新构建包，但当前的工作包不受这些问题的构建的影响。

## 处理图像

Webpack 允许我们使用 [`file-loader`](https://webpack.js.org/loaders/file-loader/) 以非常方便的方式使用图像。

简单的配置：

```js
module.exports = {
  // ...
  module: {
    rules: [
      {
        test: /\.(png|svg|jpg|gif)$/,
        use: ['file-loader']
      }
    ]
  }
  // ...
}
```

允许您在 JavaScript 中导入图像：

```js
import Icon from './icon.png'

const img = new Image()
img.src = Icon
element.appendChild(img)
```

`file-loader` 还可以处理其他资源类型，如字体、CSV 文件、XML 等。

另一个处理图像的好工具是 [`url-loader`](https://github.com/webpack-contrib/url-loader)。

此示例将加载任何小于 8KB 的 PNG 文件作为 [Data URL](https://github.com/lio-zero/blog/blob/main/%E6%B5%8F%E8%A7%88%E5%99%A8/Data%20URL.md)。

```js
module.exports = {
  // ...
  module: {
    rules: [
      {
        test: /\.png$/,
        use: [
          {
            loader: 'url-loader',
            options: {
              limit: 8192
            }
          }
        ]
      }
    ]
  }
  // ...
}
```

## 处理 SASS 代码并将其转换为 CSS

使用 `sass-loader`、`css-loader` 和 `style-loader`:

```js
module.exports = {
  // ...
  module: {
    rules: [
      {
        test: /\.scss$/,
        use: ['style-loader', 'css-loader', 'sass-loader']
      }
    ]
  }
  // ...
}
```

## 生成 Source Maps

例如，由于 webpack 捆绑了代码，因此必须使用 Source Maps（源映射）来获取对引发错误的原始文件的引用。

您告诉 webpack 使用配置的 [`devtool` 属性](https://webpack.js.org/configuration/devtool)生成源映射：

```js
module.exports = {
  /*...*/
  devtool: 'inline-source-map'
  /*...*/
}
```

`devtool` 有许多可能的值，最常用的可能是：

- `none` — 不添加源映射
- `source-map` — 非常适合生产环境，提供了一个可以最小化的独立源映射，并在包中添加了一个引用，因此开发工具知道源映射是可用的。当然，您应该配置服务器以避免发送它，并将其用于调试目的
- `inline-source-map` — 非常适合开发环境，将源映射内联为 Data URL

关于 Source Map 详细内容可以阅读 [Introduction to JavaScript Source Maps](https://developer.chrome.com/blog/sourcemaps/)。另外，推荐[深入浅出之 Source Map](https://juejin.cn/post/7023537118454480904)。

## 更多资料

- [webpack-demos](https://github.com/ruanyf/webpack-demos)
- [Webpack 减少构建时间](https://github.com/lio-zero/blog/blob/main/%E5%89%8D%E7%AB%AF/Webpack%20%E5%87%8F%E5%B0%91%E6%9E%84%E5%BB%BA%E6%97%B6%E9%97%B4.md)
- [Webpack DefinePlugin](https://github.com/lio-zero/blog/blob/main/%E5%89%8D%E7%AB%AF/Webpack%20DefinePlugin.md)
- [Webpack Watch](https://github.com/lio-zero/blog/blob/main/%E5%89%8D%E7%AB%AF/Webpack%20Watch.md)
- [使用 Webpack 编译 TypeScript](https://github.com/lio-zero/blog/blob/main/%E5%89%8D%E7%AB%AF/%E4%BD%BF%E7%94%A8%20Webpack%20%E7%BC%96%E8%AF%91%20TypeScript.md)
- [Webpack externals](https://github.com/lio-zero/blog/blob/main/%E5%89%8D%E7%AB%AF/Webpack%20externals.md)
- [使用别名缩短 Webpack 中的导入路径](https://github.com/lio-zero/blog/blob/main/%E5%89%8D%E7%AB%AF/%E4%BD%BF%E7%94%A8%E5%88%AB%E5%90%8D%E7%BC%A9%E7%9F%AD%20Webpack%20%E4%B8%AD%E7%9A%84%E5%AF%BC%E5%85%A5%E8%B7%AF%E5%BE%84.md)
- [构建模块打包器](https://github.com/lio-zero/blog/blob/main/%E6%89%8B%E5%86%99%E7%B3%BB%E5%88%97/%E6%9E%84%E5%BB%BA%E6%A8%A1%E5%9D%97%E6%89%93%E5%8C%85%E5%99%A8.md)
