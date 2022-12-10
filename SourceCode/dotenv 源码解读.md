# dotenv 源码解读

[dotenv](https://github.com/motdotla/dotenv) 是一个方便管理环境变量的库，用于将环境变量从 `.env` 文件加载到 `process.env` 中。

我们将其导入到项目入口文件的开头：

```js
dotenv.config()
```

当项目启动时，dotenv 将 `.env` 文件中的环境变量加载到运行时的环境中，可以使用 `process.env` 去获取。

这样可以帮助我们将环境变量与应用程序代码分离，更容易地进行环境配置和管理，从而提高环境变量的管理效率和应用程序的安全性和可维护性。

下面我们来看看 dotenv 是如何实现将 `.env` 的环境变量添加到 `process.env` 上的。

## 源码分析

dotenv 的源码主要包括以下三个部分：

- 加载 `.env` 文件
- 解析 `.env` 文件中的变量
- 将解析出的变量加载到 `process.env` 中

通过查看 [`lib/main.js`](https://github.com/motdotla/dotenv/blob/master/lib/main.js) 文件，我们可以发现它只导出了两个函数：`config` 和 `parse`。

我们先来看一下源码：

```js
// 简化后
const path = require('path')
const fs = require('fs')

function _resolveHome(envPath) {
  return envPath[0] === '~'
    ? path.join(os.homedir(), envPath.slice(1))
    : envPath
}

function config(options) {
  // 获取 .env 文件的绝对路径
  let dotenvPath = path.resolve(process.cwd(), '.env')
  let encoding = 'utf8'
  const debug = Boolean(options && options.debug)
  const override = Boolean(options && options.override)

  // 可选操作
  if (options) {
    // 自定义 .env 文件的路径
    if (options.path != null) {
      dotenvPath = _resolveHome(options.path)
    }
    // 自定义 .env 文件的编码方式，默认使用 utf8
    if (options.encoding != null) {
      encoding = options.encoding
    }
  }

  try {
    // Specifying an encoding returns a string instead of a buffer
    // 使用 parse 函数将读取到的 key-value 解析为对象，并返回
    const parsed = parse(fs.readFileSync(dotenvPath, { encoding }))

    /**
     * 遍历对象，判断 process.env 对象是否已存在相同的 key（环境变量）
     * - 不存在则将其添加到 process.env 中
     * - 存在则不添加到 process.env 中，但如果用户传入可选参数 options.override 为 true，那么将覆盖已存在的环境变量
     */
    Object.keys(parsed).forEach(function (key) {
      if (!Object.prototype.hasOwnProperty.call(process.env, key)) {
        process.env[key] = parsed[key]
      } else {
        if (override === true) {
          process.env[key] = parsed[key]
        }

        if (debug) {
          // ...
        }
      }
    })

    return { parsed }
  } catch (e) {
    // ...
  }
}
```

> 这里省略了一些细节，感兴趣可自行了解。

`parse` 函数则用于将 `.env` 文件中的内容解析为键值对，并将它们存储在对象中返回。

```js
const LINE =
  /(?:^|^)\s*(?:export\s+)?([\w.-]+)(?:\s*=\s*?|:\s+?)(\s*'(?:\\'|[^'])*'|\s*"(?:\\"|[^"])*"|\s*`(?:\\`|[^`])*`|[^#\r\n]+)?\s*(?:#.*)?(?:$|$)/gm

function parse(src) {
  const obj = {}

  // Convert buffer to string
  let lines = src.toString()

  // Convert line breaks to same format
  lines = lines.replace(/\r\n?/gm, '\n')

  let match
  while ((match = LINE.exec(lines)) != null) {
    const key = match[1]

    // Default undefined or null to empty string
    let value = match[2] || ''

    // Remove whitespace
    value = value.trim()

    // Check if double quoted
    const maybeQuote = value[0]

    // Remove surrounding quotes
    value = value.replace(/^(['"`])([\s\S]*)\1$/gm, '$2')

    // Expand newlines if double quoted
    if (maybeQuote === '"') {
      value = value.replace(/\\n/g, '\n')
      value = value.replace(/\\r/g, '\r')
    }

    // Add to object
    obj[key] = value
  }

  return obj
}
```

`parse` 函数基本上每行都有注释，dotenv 文档也解释了 parse 解析遵循的规则：[What rules does the parsing engine follow?](https://github.com/motdotla/dotenv#what-rules-does-the-parsing-engine-follow)。

## 总结

我们总结一下这两个函数所做的工作：

- 首先，获取 `.env` 文件路径。（这里忽略 `path`、`debug`、`encoding`、`override` 的解释）
- 接着，使用 `fs` 模块读取 `.env` 文件中的内容，并将其传递给 `parse` 函数。`parse` 函数会将 `.env` 文件中的内容解析为键值对形式的对象。（具体解释，请看上面代码部分注释）
- 最后，`config` 函数将解析出的变量附加到 `process.env` 中。
