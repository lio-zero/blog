# Webpack DefinePlugin

Webpack 的 [`DefinePlugin()`](https://webpack.js.org/plugins/define-plugin/) 函数允许你用另一个令牌替换编译代码中的给定令牌。一个常见的用例是，当您不能直接使用 `.env` 文件时，使用它来定义环境变量。

假设我们的 `.env` 文件有一个 `KEY` 值：

```js
KEY = 123456
```

我们使用 [`dotenv`](https://www.npmjs.com/package/dotenv) 模块，并配置如下 webpack：

```js
'use strict'

const webpack = require('webpack')
require('dotenv').config()

const compiler = webpack({
  entry: {
    app: `${__dirname}/src/main.js`
  },
  output: {
    path: `${__dirname}/public`,
    filename: 'bundle.js'
  },
  plugins: [
    new webpack.DefinePlugin({
      __KEY: `${process.env.KEY}`
    })
  ]
})
```

编译前：

```js
console.log(__KEY)
```

编译后：

```js
/******/ ;() => {
  // webpackBootstrap
  /******/ 'use strict'
  var __webpack_exports__ = {} // CONCATENATED MODULE: ./app.js

  /***/ ;() => {
    eval(
      "console.log('123456')\r\n\n\n//# sourceURL=webpack://webpack-DefinePlugin/./app.js?"
    )

    /***/
  }

  /******/
}
```

## 切换环境

另一个有用的技巧是使用 `DefinePlugin()` 在开发和生产服务器 URL 之间切换。例如，假设您希望根据 `NODE_ENV` 将前端发出请求的服务器切换到另一个服务器。下面是使用 `DefinePlugin()` 的方法：

```js
new Webpack.DefinePlugin({
  URL:
    process.env.NODE_ENV === 'development'
      ? `'http://localhost:3000'`
      : `'https://api.myapp.com'`
})
```
