# Node.js path 模块

[`path` 模块](https://nodejs.org/api/path.html)是一个内置模块，可帮助您以独立于操作系统的方式使用文件系统路径。如果要构建支持 OSX、Linux 和 Windows 的 CLI 工具，则 `path` 模块是必不可少的。

即使您正在构建一个只在 Linux 上运行的后端服务，`path` 模块仍然有助于在操作路径时避免边缘情况。

下面我们来介绍 `path` 模块的基本用法，以及为什么您应该使用 `path` 模块而不是将路径操纵成字符串。

## 在 Node 中使用 `path` 模块

`path` 模块中最常用的方法是 `path.join()`。该方法将一个或多个路径段合并为一个字符串，如下所示：

```js
const path = require('path')

path.join('/path', 'to', 'test.txt') // '/path/to/test.txt'
```

您可能想知道为什么要使用 `path.join()` 方法而不是字符串拼接。

```js
'/path' +
  '/' +
  'to' +
  '/' +
  'test.txt'[('/path', 'to', 'test.txt')] // '/path/to/test.txt'
    .join('/') // '/path/to/test.txt'
```

原因主要有两个：

- **对于 Windows 支持**。Windows 使用反斜杠（`\`）而不是正斜杠（`/`）作为路径分隔符。`path.join()` 会为我们处理此问题。因为 `path.join('data', 'test.txt')` 在 Linux 和 OSX 以及 Windows 上都会返回 `'data/test.txt'`。
- **用于处理边缘情况**。使用文件系统路径时，会弹出许多边缘情况。例如，如果您尝试手动连接两个路径，您可能会意外地得到重复的路径分隔符。`path.join()` 方法为我们处理开头和结尾的斜杠，如下所示：

```js
path.join('data', 'test.txt') // 'data/test.txt'
path.join('data', '/test.txt') // 'data/test.txt'
path.join('data/', 'test.txt') // 'data/test.txt'
path.join('data/', '/test.txt') // 'data/test.txt'
```

## 常用的 path 方法

`path` 模块还具有几个用于提取路径组件的方法，例如文件扩展名或目录。

`path.extname()` 方法以字符串形式返回文件扩展名：

```js
path.extname('/path/to/test.txt') // '.txt'
```

就像连接两条路径一样，获取文件扩展名比最初看起来要复杂。

如果 `path` 以 `.` 为结尾，将返回 `.`。如果文件无扩展名，又不以 `.` 结尾，或文件没有扩展名，将返回空值。

```js
path.extname('/path/to/index.') // '.'
path.extname('/path/to/README') // ''
path.extname('/path/to/.gitignore') // ''
```

`path` 模块还有 `path.basename()` 和 `path.dirname()` 方法，分别获取文件名（包括扩展名）和目录。

```js
path.basename('/path/to/test.txt') // 'test.txt'
path.dirname('/path/to/test.txt') // '/path/to'
```

`path.parse()` 方法返回一个对象，该对象包含分为五个不同组合的路径，包括扩展名和目录。`path.parse()` 方法也是不带任何扩展名获取文件名的方法。

```js
path.parse('/path/to/test.txt')

/*
{
  root: '/',
  dir: '/path/to',
  base: 'test.txt',
  ext: '.txt',
  name: 'test'
}
*/
```

## 使用 `path.relative()`

像 `path.join()` 和 `path.extname()` 这样的方法涵盖了大多数使用文件路径的用例。但是 `path` 模块有几个更高级的方法，例如 `path.relative()`。

`path.relative(from, to)` 方法根据当前工作目录返回从 `from` 到 `to` 的相对路径。 如果 `from` 和 `to` 都解析为相同的路径（在分别调用 `path.resolve()` 之后），则返回零长度字符串。

```js
// 返回相对于第一条路径的第二条路径的路径
path.relative('/app/views/home.html', '/app/layout/index.html') // '../../layout/index.html'
```

如果给定了相对于一个目录的路径，但需要相对于另一个目录的路径，则 `path.relative()` 方法非常有用。例如，流行的文件系统监视库 [Chokidar](https://www.npmjs.com/package/chokidar) 提供了相对于监视目录的路径。

```js
const watcher = chokidar.watch('mydir')

// 如果用户添加 mydir/path/to/test.txt，则会打印 mydir/path/to/test.txt
watcher.on('add', (path) => console.log(path))
```

这就是为什么大量的使用 [Chokidar](https://github.com/paulmillr/chokidar) 工具。如常见的 Gatsby 或 webpack，其在内部也大量使用 `path.relative()` 方法。

例如，[Gatsby 使用 `path.relative()` 方法帮助同步静态文件目录](https://github.com/gatsbyjs/gatsby/blob/54d4721462b9303fed723fdcb15ac5d72e103778/packages/gatsby/src/utils/get-static-dir.ts#L50-L56)。

```js
export const syncStaticDir = (): void => {
  const staticDir = nodePath.join(process.cwd(), `static`)
  chokidar
    .watch(staticDir)
    .on(`add`, (path) => {
      const relativePath = nodePath.relative(staticDir, path)
      fs.copy(path, `${process.cwd()}/public/${relativePath}`)
    })
    .on(`change`, (path) => {
      const relativePath = nodePath.relative(staticDir, path)
      fs.copy(path, `${process.cwd()}/public/${relativePath}`)
    })
}
```

现在，假设用户向 `static` 目录添加了一个新文件 `main.js`。Chokidar 调用 `on('add')` 事件处理程序，路径设置为 `static/main.js`。但是，当您将文件复制到 `/public` 时，不需要额外的 `static/`。

调用 `path.relative('static', 'static/main.js')` 返回 `static/main.js` 相对于 `static` 的路径，这正是您想要将 `static` 的内容复制到 `public` 的路径。

## 跨操作系统路径和 URL

默认情况下，`path` 模块会根据 Node 进程运行的操作系统自动在 POSIX（OSX、Linux）和 Windows 模式之间切换。

但是，`path` 模块确实可以在 POSIX 上使用 Windows `path` 模块，反之亦然。`path.posix` 和 `path.win32` 属性分别包含 `path` 模块的 POSIX 和 Windows 版本。

```js
// 返回 'path\to\test.txt'，与操作系统无关
path.win32.join('path', 'to', 'test.txt')

// 返回 'path/to/test.txt'，与操作系统无关
path.posix.join('path', 'to', 'test.txt')
```

在大多数情况下，根据检测到的操作系统自动切换 path 模块是正确的行为。但是，使用 `path.posix` 和 `path.win32` 属性对于总是希望输出 Windows 或 Linux 样式路径的测试或应用程序可能会有所帮助。

例如，一些应用程序使用 `path.join()` 和 `path.extname()` 等方法处理 URL 路径。

```js
// 'https://api.mydomain.app/api/v2/me'
'https://api.mydomain.app/' + path.join('api', 'v2', 'me')
```

这种方法适用于 Linux 和 OSX，但如果有人试图将您的应用程序部署到一些无服务器上会发生什么？

你最终会得到 `https://api.mydomain.app/api\v2\me`，这不是有效的 URL！如果使用 `path` 模块操作 URL，则应使用 `path.posix`。
