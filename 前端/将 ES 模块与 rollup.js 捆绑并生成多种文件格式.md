# 将 ES 模块与 rollup.js 捆绑并生成多种文件格式

## 模块打包器

Rollup.js 是一个模块打包器。它接受输入文件，并将它们组合成一种或多种格式的单个输出文件。

这里起一个简单的 rollup-demo 项目，其中一个 `helpers.js` 文件，如下所示。

```js
let answer = 42

let getTheAnswer = function () {
  return answer
}

export default getTheAnswer
```

另一个 `index.js` 从 `helper.js` 中导入内容，如下所示。

```js
import getTheAnswer from './modules/helpers.js'

let name = 'O.O'

alert('The answer is ' + getTheAnswer())
```

我们可以使用 rollup.js 来输出一个 `main.js` 文件，该文件导入所有必需的文件和函数，并将它们限定在 IIFE 中，如下所示。

```js
;(function () {
  'use strict'

  let answer = 42
  let getTheAnswer = function () {
    return answer
  }

  alert('The answer is ' + getTheAnswer())
})()
```

这避免了使用原生 ES 模块时多个 HTTP 请求导致的性能问题，还允许您在不支持 ES 模块的旧浏览器中运行脚本。

让我们看看它是如何工作的。

## 如何使用 rollup.js

我们先在项目上安装 rollup 为开发依赖：

```bash
npm i rollup -D
```

如果你愿意，你可以完全从命令行运行 rollup.js。但是，创建一个配置文件要容易得多。

在项目根目录创建一个名为 `rollup.config.js`。这是我们将告诉 rollup.js 打包哪些文件以及如何输出它们的地方。

在我们的例子中，我们将入口文件指定为 `index.js` 文件，并将其输出为一个名为 `main.js` 的文件。任何在 `index.js` 中的导入也会被自动拉进来。我们希望将文件作为 `iife` 输出。

```js
export default {
  input: 'index.js',
  output: [
    {
      file: 'dist/main.js',
      format: 'iife'
    }
  ]
}
```

要运行 rollup.js，我们可以在命令行中输入 `npx rollup --config`。

- `npx` 它会直接去 `node_modules/.bin` 找到命令运行。如果您在本地安装了 rollup，那么您可以忽略它。
- `rollup` 关键字运行 rollup.js
- `--config` 告诉它对所有选项和设置使用我们的配置文件。

在较大的开发项目中，创建 npm 命令来运行常见任务很有帮助。

在 `package.json` 文件中，我们添加一个 `scripts` 对象。并添加一个 `"build": "rollup --config"`：

```json
{
  "scripts": {
    "build": "rollup --config"
  }
}
```

运行 `npm run build`，将生成一个 `main.js` 文件，内容如下：

```js
;(function () {
  'use strict'

  let answer = 42

  let getTheAnswer = function () {
    return answer
  }

  alert('The answer is ' + getTheAnswer())
})()
```

## 使用输出文件

当你要使用新的 `main.js` 文件时，你可以跳过 `script` 元素上的 `type="module"` 属性。

打包的文件不是一个模块。它是一个普通的老式传统脚本，使用模块打包器从 ES 模块构建而来。

## 生成多种格式

如何从你的包中创建多种文件格式。

例如，假设您有一个 JS 库，您想要以浏览器支持的格式（IIFE 或显示模块模式）分发，以及一个带有 ES 模块导出的文件，以及另一个用于 Node.js 的文件。

Rollup.js 使您可以非常轻松地编写一次代码库，并以多种格式为各种用户提供它。

### `rollup.config.js` 导出文件中的数组

在 `rollup.config.js` 文件中，我们导出了一个设置对象，如下所示。

```js
export default {
  input: 'index.js',
  output: [
    {
      file: 'dist/main.js',
      format: 'iife'
    }
  ]
}
```

但是 rollup.js 还允许您导出一组选项，并将遍历每个选项，并基于它生成一个文件。

在 `rollup.config.js` 文件中，让我们创建一个要导出为 `formats` 的数组。在本例中，让我们创建一个 IIFE （`iife`），一个 ES 模块（`es`），将所有组件打包到一个文件中，以及一个供 Node 用户使用的 Common JS 文件（`cjs`）。

```js
let formats = ['iife', 'es', 'cjs']
```

接下来，我们将使用 `Array.map()` 方法遍历每个 `format`，为其创建一个配置，并生成一个新的配置数组。

```js
let formats = ['iife', 'es', 'cjs']

export default formats.map((format) => {
  return {
    input: 'index.js',
    output: {
      file: 'dist/main.js',
      format: format
    }
  }
})
```

通过此设置，我们将创建三个不同的导出，每种格式一个。但是，因为它们都导出到 `main.js`，所以每个都会覆盖它之前的那个。

### 创建唯一的文件名

我们可以使用 `format` 作为 `file` 名称的一部分。

我喜欢做的一件事是使用三元运算符来检查 `format` 是否为 `iife`。如果是这样，我将其作为主文件，并将 `format` 名称添加到其余文件中。

```js
export default formats.map((format) => {
  return {
    input: 'index.js',
    output: {
      file: `dist/main${format === 'iife' ? '' : `.${format}`}.js`,
      format: format
    }
  }
})
```

现在，当您运行 `npm run build` 时，您将获得三个具有三种不同输出格式的文件：`main.js`、`main.es.js` 和 `main.cjs.js`。
