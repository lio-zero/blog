# 使用别名缩短 Webpack 中的导入路径

假设您正在使用 [Webpack](https://webpack.js.org/) 捆绑您的应用程序。如果您认为在导入给定文件时使用相对路径很难看，并且在更改目录结构时很难维护：

```js
import { formatDate } from '../../../root/lib/formatDate'
```

那么使用别名就是解决方案之一。

别名（`alias`）是 webpack 的一种方便的方式，可以减少导入常用模块的时间和按键次数。除此之外，通过别名的方式来映射一个路径，从而减少⽂件搜索范围可以优化 webpack 的打包速度。

您将需要 node.js 中包含的 `path` 模块，因为这是告诉 webpack 在哪里查找这些特定文件的方式。

使用 `resolve.alias` 属性，您可以为频繁导入的模块定义别名。下面是一个例子：

```js
const path = require('path')

function resolve(dir) {
  return path.join(__dirname, dir)
}

module.exports = {
  resolve: {
    alias: {
      Library: resolve('root/lib/'),
      Single: resolve('root/test.js')
    }
  }
}
```

因此，现在当您想要从库模块导入文件时，可以使用 `import { file } from 'Library/fileLocation`，或者如果您将文件包含在别名中，则从 `import { test } from 'Single'`。

## 使用别名作为布尔值

如果您已经通过 CDN 为您的应用程序加载了一个库，并且将其作为依赖项，那么这将在您的应用程序中产生冲突。因此，您可以在 `resolve.alias` 属性中列出冲突模块的路径，并将其设置为 `false` 以解决冲突。

```js
module.exports = {
  resolve: {
    alias: {
      'path/to/ignored/module': false
    }
  }
}
```

## 使用 `$` 进行精确匹配

您可以在别名定义中添加尾随 `$`，这样可以确保如果路径不完全匹配，则强制执行错误。

```js
const path = require('path')

function resolve(dir) {
  return path.join(__dirname, dir)
}

module.exports = {
  resolve: {
    alias: {
      Single$: resolve('root/test.js')
    }
  }
}
```

所以现在当你尝试导入 **test.js** 时：

```js
import Test from 'Single' // success
import Test2 from 'Single/test.js' // error，root/test.js 无效
```
