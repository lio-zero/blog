# Webpack 减少构建时间

> Webpack 构建项目的速度很大程度上取决于项目的复杂度和电脑配置。确保项目在有足够的磁盘空间和良好的处理器情况下运行。

除此之外，我们可以通过一些配置来提升 webpack 的构建速度。webpack 的 [Build Performance](https://webpack.docschina.org/guides/build-performance/) 章节提供了一些提高构建/编译性能的方法。本文将根据 webpack 提供的优化建议来减少项目构建时间。

## 保持最新的 Webpack、Node 和包管理器

使用最新的 webpack 版本（新优化），并且与 Node 版本同步，重点是 `npm/yarn/pnpm` 包管理器，较新的版本创建更高效的模块树并提高解析速度。

## 优化 Loader 的文件搜索范围

通过使用 `include` 和 `exclude` 字段，仅将 loader 应用在实际需要将其转换的模块：

```js
module.exports = {
  module: {
    rules: [
      {
        test: /\.js$/,
        loader: 'babel-loader',
        include: path.resolve(__dirname, 'src'),
        exclude: /node_modules/
      }
    ]
  }
}
```

以上示例中，`test` 设置仅在 JS 文件时才使用 `babel-loader`，`include` 仅在 `src` ⽂件夹下查找，`exclude` 排除 `node_modules` 目录。

另外，将 Babel 编译过的文件缓存起来，下次只需要编译更改过的文件即可，这样可以大幅度加快打包时间。

```js
loader: 'babel-loader?cacheDirectory=true'
```

## 别名 — `resolve.alias`

通过别名的方式来映射一个路径，能让 webpack 更快找到路径：

```js
const path = require('path')

function resolve(dir) {
  return path.join(__dirname, dir)
}

module.exports = {
  resolve: {
    alias: {
      '@': resolve('src')
    }
  }
}
```

## 解析

减少以下方法条目数量，因为他们会增加文件系统调用的次数。（层级不要过深）

- [`resolve.modules`](https://webpack.docschina.org/configuration/resolve/#resolvemodules) 告诉 webpack 解析模块时应该搜索的目录。
- [`resolve.extensions`](https://webpack.docschina.org/configuration/resolve/#resolveextensions) 尝试按顺序解析这些后缀名。如果有多个文件有相同的名字，但后缀名不同，webpack 会解析列在数组首位的后缀的文件 并跳过其余的后缀。
- [`resolve.mainFiles`](https://webpack.docschina.org/configuration/resolve/#resolvemainfiles) 解析目录时要使用的文件名。
- [`resolve.descriptionFiles`](https://webpack.docschina.org/configuration/resolve/#resolvedescriptionfiles) 用于描述的 JSON 文件。

如果不使用 [symlinks](https://classic.yarnpkg.com/en/package/symlinks)（例如 `npm link` 或 `yarn link`），可以设置：

```js
resolve: {
  symlinks: false
}
```

如果你使用自定义 resolve plugin 规则，并且没有指定 context 上下文，可以设置：

```js
resolve: {
  cacheWithContext: false
}
```

## 减少项目体积

减少编译结果的整体大小，以提高构建性能。尽量保持 chunk 体积小

- 使用数量更少/体积更小的 library（例如：`moment -> day.js`、`lodash -> lodash/es`）。
- 在多页面应用程序中使用 [SplitChunksPlugin](https://webpack.docschina.org/plugins/split-chunks-plugin#root)，并开启 `async` 模式。
- 移除未引用代码 — 涉及到了 [Tree Shaking](https://github.com/lio-zero/blog/blob/main/%E5%89%8D%E7%AB%AF/Tree%20Shaking.md)。
- 只编译你当前正在开发的那些代码（缓存）。

## 最小化 entry chunk

确保在生成 entry chunk 时，尽量减少其体积以提高性能。将 `optimization.runtimeChunk` 设置为 `true` 或 `'multiple'`，会为每个入口添加一个只含有 runtime 的额外 chunk。所以它的生成代价较低：

```js
module.exports = {
  // ...
  optimization: {
    runtimeChunk: true
  }
}
```

## 解析构建资源

webpack 提供了 [`thread-loader`](https://webpack.docschina.org/loaders/thread-loader) 允许我们可以将耗时的 loader 放置在独立的线程下运行：

```js
module.exports = {
  module: {
    rules: [
      {
        test: /\.js$/,
        include: path.resolve('src'),
        use: [
          'thread-loader' // 注意，这里需要放置在第一位
          // 后面放置耗时的 loader （例如 babel-loader）
        ]
      }
    ]
  }
}
```

配置 `thread-loader`：

```js
use: [
  {
    loader: 'thread-loader',
    options: {
      workers: 2,
      workerParallelJobs: 50,
      workerNodeArgs: ['--max-old-space-size=1024'],
      poolRespawn: false,
      poolTimeout: 2000,
      poolParallelJobs: 50,
      name: 'my-pool'
    }
  }
]
```

详细的介绍，查看[文档](https://webpack.docschina.org/loaders/thread-loader#examples)。

> [`happypack`](https://www.npmjs.com/package/happypack) 插件已经未维护好几年，使用 `thread-loader` 替换。

## 开启多进程压缩代码

webpack v5 提供了开箱即用最新版本的 [`terser-webpack-plugin`](https://webpack.docschina.org/plugins/terser-webpack-plugin)，其默认的生产环境下缩小您的代码。但如果需要自定义配置，仍需要安装它。

```js
const TerserPlugin = require('terser-webpack-plugin')

module.exports = {
  optimization: {
    minimize: true,
    minimizer: [new TerserPlugin()]
  }
}
```

其中，你可以配置 [`parallel`](https://webpack.docschina.org/plugins/terser-webpack-plugin#parallel) 属性，它使用多进程并发运行以提高构建速度。

```js
new TerserPlugin({
  parallel: true,
  cache: true
})
```

> **注意**，这里启用了 `cache` 意味着只有当现有文件发生新更改时，terser 才会压缩它们，提升二次构建速度。

## HardSourceWebpackPlugin

[`HardSourceWebpackPlugin`](https://github.com/mzgoddard/hard-source-webpack-plugin) 用于为模块提供中间缓存步骤，提升二次构建速度：

```js
const HardSourceWebpackPlugin = require('hard-source-webpack-plugin')

module.exports = {
  plugins: [new HardSourceWebpackPlugin()]
}
```

缓存默认的存放路径是：`node_modules/.cache/hard-source`。

> **注意**：首次构建时间并没有太大变化，它只在后续的的构建中有所提升，而且建议不要在开发环境中使用它，它并不理想，但在生产环境中会得到很大提升。另外，它已经有好几年未维护。

## 避免额外的优化步骤

webpack 通过执行额外的算法任务，来优化输出结果的体积和加载性能。这些优化适用于小型代码库，但是在大型代码库中却非常耗费性能：

```js
module.exports = {
  // ...
  optimization: {
    removeAvailableModules: false,
    removeEmptyChunks: false,
    splitChunks: false
  }
}
```

## 输出结果不携带路径信息

webpack 会在输出的 bundle 中生成路径信息。然而，在打包数千个模块的项目中，这会导致造成垃圾回收性能压力。在 `options.output.pathinfo` 设置中关闭：

```js
module.exports = {
  // ...
  output: {
    pathinfo: false
  }
}
```

## 持久化缓存

添加 [`cache`](https://webpack.js.org/configuration/cache/#cache)，持久缓存生成的 webpack 模块和 chunks，以提高构建速度。

```js
module.exports = {
  //...
  cache: true
}
```

对于开发环境使用 `true`，对于生产环境使用 `false`。

## 其他

- 每个 loader 都有一个启动时间，去除无用的 loader。
- webpack 会自动开启 [**tree-shaking** 优化](https://webpack.docschina.org/blog/2020-10-10-webpack-5-release/#major-changes-optimization)，去除无用代码
- 使用 `speed-measure-webpack-plugin` 插件分析每个 loader、plugin 的耗时：

```js
const SpeedMeasurePlugin = require('speed-measure-webpack-plugin')

const smp = new SpeedMeasurePlugin()

const webpackConfig = smp.wrap({
  plugins: [new MyPlugin(), new MyOtherPlugin()]
})
```

- 使用 [`webpack-bundle-analyzer`](https://www.npmjs.com/package/webpack-bundle-analyzer) 分析包的体积：

```js
const BundleAnalyzerPlugin =
  require('webpack-bundle-analyzer').BundleAnalyzerPlugin

module.exports = {
  plugins: [new BundleAnalyzerPlugin()]
}
```

- [DllPlugin](https://webpack.docschina.org/plugins/dll-plugin) 表示动态链接库。这里不展开解释，并且建议有一些文章建议不在使用它，详细可以查看[辛辛苦苦学会的 webpack dll 配置，可能已经过时了](https://juejin.cn/post/6844903952140468232)。
