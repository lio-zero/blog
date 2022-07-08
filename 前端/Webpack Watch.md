# Webpack Watch

通常，当您在开发阶段运行 Webpack 时，您希望在 [`watch` 模式](https://webpack.js.org/configuration/watch/)下运行它。这将配置 Webpack 来监视项目中的文件更改，并在文件更改时重新编译。换句话说，你不必每次都手动重新运行 Webpack。

例如，假设您有以下 `webpack.config.js` 文件。它需要一个 `app.js` 文件，并将其编译为 `./bin/app.min.js`。

```js
module.exports = {
  mode: 'development',
  entry: {
    app: `${__dirname}/app.js`
  },
  target: 'web',
  output: {
    path: `${__dirname}/bin`,
    filename: '[name].min.js'
  }
}
```

假设 `app.js` 包含一个简单的打印语句：

```js
console.log('Hello, world')
```

现在，运行 `./node_modules/.bin/webpack --watch`，您应该会看到下面的输出。确保你同时安装了 Webpack 和 Webpack CLI。

![watch](https://upload-images.jianshu.io/upload_images/18281896-4eb27b7e0768f918.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们修改 `app.js`：

```js
console.log('Hello, world !')
```

Webpack 将检测到更改并重新编译：

![watch](https://upload-images.jianshu.io/upload_images/18281896-66433f90342ae6f5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 启用 `watch` 模式的其他方法

您还可以从 Webpack 配置文件启用监视模式：

```js
module.exports = {
  mode: 'development',
  watch: true, // 启用 watch 模式
  entry: {
    app: `${__dirname}/app.js`
  },
  target: 'web',
  output: {
    path: `${__dirname}/bin`,
    filename: '[name].min.js'
  }
}
```

然而，这种方法通常是一个糟糕的选择，因为如果您在 CI/CD 工具或 `git commit` 钩子中进行编译，您不想在 `watch` 模式下运行 Webpack。你应该使用 `--watch` 启用 `watch` 模式，除非您确定永远不希望在没有 `watch` 的情况下运行 Webpack。
