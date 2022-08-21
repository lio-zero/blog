# Node.js 文件系统模块

## 了解如何使用 Node.js 中的 `fs` 模块

[文件系统模块](https://nodejs.org/api/fs.html)（简称 `fs`）允许我们访问计算机上的文件系统并与之交互。

使用 `fs` 模块，我们可以执行以下操作：

- 创建文件和目录
- 修改文件和目录
- 删除文件和目录
- 读取文件和目录的内容

> **建议**：文件系统模块比较复杂，建议查看[官方文档](https://nodejs.org/docs/latest-v16.x/api/fs.html)，你可以看到所有的方法。

本文将向您介绍最常见和最有用的 `fs` 方法。事不宜迟，让我们看看这些方法是什么。

## 如何使用 `fs`

文件系统模块是一个核心的 Node.js 模块。这意味着我们不必安装它。我们唯一需要做的就是将 `fs` 模块导入到自己的文件中。

因此，在文件顶部添加：

```js
const fs = require('fs')
```

现在，我们可以使用前缀 `fs` 从文件系统模块调用任何方法。

或者，我们可以只从 `fs` API 导入所需的方法，如下所示：

```js
const { writeFile, readFile } = require('fs')
```

> **注意**：为了方便起见，我们还需要导入 `path` 模块。它是另一个核心 Node.js 模块，它允许我们使用文件和目录路径。

导入 `fs` 模块后，在文件中添加：

```js
const path = require('path')
```

使用文件系统模块时，`path` 模块不是必需的。但它对我们有很大的帮助！

## 同步与异步

需要注意的是，默认情况下，所有 `fs` 方法都是异步的。但是，我们可以通过在方法末尾添加 `Sync` 来使用同步版本。

例如，`writeFile` 方法改为 `writeFileSync`。

同步方法有两个缺点：

- 同步方法同步的执行代码，因此它们阻塞了主线程。例如：我使用 `fs.readdirSync` 同步读取目录下的所有文件，它将阻塞后面代码的运行，直到读取目录完成。阻塞 Node.js 中的主线程被认为是不好的做法，我们不应该这么做
- 还有，同步代码需要使用 `try...catch` 捕获错误

因此，以下都使用文件系统模块中的异步方法。

## 写入文件

要从 Node.js 应用程序写入文件，请使用 `writeFile` 方法。

`writeFile` 方法至少接受以下参数：

- 文件名
- 内容
- 回调

如果指定的文件已经存在，它会将旧内容替换为您作为参数提供的内容。如果指定的文件不存在，则创建一个新文件。

导入 `fs` 和 `path` 模块后，在文件中编写以下代码：

```js
fs.writeFile('content.txt', 'All work and no play makes Jack a dull boy!', (err) => {
  if (err) throw err

  process.stdout.write('创建成功!')
})
```

上面的代码创建了一个名为 `content.txt` 的新文件，并添加了文本 `All work and no play makes Jack a dull boy!` 作为内容。如果存在任何错误，回调函数将抛出该错误。否则，它将向控制台输出文件创建成功。

假设您将文件命名为 `fs-Demo.js`，请转到终端并运行命令 `node fs-Demo.js`。运行该命令后，您应该会看到具有指定内容的新文件。

`writeFile` 还有其他变体，例如：

- `fs.writeFileSync` — 同步写入文件
- `fs.promises.writeFile` — 使用基于 Promise 的 API 写入文件

## 从文件中读取

在读取文件之前，需要创建并存储文件的路径。`path` 模块的路径在这里很方便。

使用 `join` 模块中的 `path` 方法，您可以创建文件路径，如下所示：

```js
const filePath = path.join(process.cwd(), 'content.txt')
```

第一个参数 `process.cwd()` 返回当前工作目录。现在您已经有了文件路径，可以读取文件的内容了。

在文件中编写以下代码：

```js
fs.readFile(filePath, (error, content) => {
  if (error) throw error

  process.stdout.write(content)
})
```

`readFile` 方法至少接受两个参数：

- 文件的路径
- 回调

如果有错误，它会抛出一个错误。否则，它会在终端中输出文件内容。

如果您转到终端并运行命令 `node fs-Demo`，您应该会在终端中看到文件内容。

`readFile` 还有其他变体，例如：

- `fs.readFileSync` — 同步写入文件
- `fs.promises.readFile` — 使用基于 Promise 的 API 写入文件

## 读取目录中的文件列表

读取目录中的文件列表与读取文件内容非常相似。但是，不是传递文件路径，而是传递当前工作目录（我们可以传递任何其他目录）。

然后，传递一个回调函数来处理响应。在文件中编写以下代码：

```js
fs.readdir(process.cwd(), (error, files) => {
  if (error) throw error

  console.log(files)
})
```

到目前为止，您只使用 `process.stdout.write` 将内容输出到终端。但是，您可以简单地使用 `console.log`，就像上面的代码片段一样。

如果运行该应用程序，我们应该会得到一个包含目录中所有文件的数组。

它同有一个同步 `readdirSync` 方法。

## 删除文件

文件系统模块有一种方法，允许您删除文件。但是，需要注意的是，它只适用于文件，不适用于目录。

当以文件路径作为参数调用 `unlink` 方法时，它将删除该文件。将以下代码段添加到文件中：

```js
fs.unlink(filePath, (error) => {
  if (error) throw error

  console.log('文件已删除!')
})
```

如果您重新运行代码，您的文件将被删除！

## 创建目录

我们可以使用 `mkdir` 方法异步创建目录。在文件中编写以下代码：

```js
fs.mkdir(`${process.cwd()}/myFolder/secondFolder`, { recursive: true }, (err) => {
  if (err) throw err

  console.log('已成功创建文件夹!')
})
```

首先，要在当前工作目录中创建一个新文件夹。如前所述，您可以使用 `cwd()` 方法从 `process` 对象获取当前工作目录。

然后，传递要创建的一个或多个文件夹。但是，这并不意味着您必须在当前工作目录中创建新文件夹。你可以在任何地方创建它们。

现在，第二个参数是递归选项。如果未将其设置为 `true`，则无法创建多个文件夹。如果将 `recursive` 选项设置为 `false`，上述代码将给出一个错误。**试试看，多尝试总是好的！**

但是，如果您只想创建一个文件夹，则无需将 `recursive` 选项设置为 `true`。

以下代码可以正常工作！

```js
fs.mkdir(`${process.cwd()}/myFolder`, (err) => {
  if (err) throw err

  console.log('已成功创建文件夹!')
})
```

因此，我想强调使用 `recursive`。如果要在文件夹中创建文件夹，则需要将其设置为 `true`。它将创建所有文件夹，即使它们不存在。

另一方面，如果您只想创建一个文件夹，可以将其保留为 `false`。

## 删除目录

删除目录的逻辑类似于创建目录。如果您查看为创建目录而编写的代码和下面的代码，您会发现相似之处。

因此，在文件中编写以下代码：

```js
fs.rmdir(`${process.cwd()}/myFolder/`, { recursive: true }, (err) => {
  if (err) throw err

  console.log('已成功删除文件夹!')
})
```

使用文件系统模块中的 `rmdir` 方法，并传递以下参数：

- 要删除的目录
- 递归属性
- 回调

如果将 `recursive` 属性设置为 `true`，它将删除文件夹及其内容。请务必注意，如果文件夹中包含内容，则**需要将其设置为 `true`**。否则，您将得到一个错误。

以下代码段仅在文件夹为空时有效：

```js
fs.rmdir(`${process.cwd()}/myFolder/`, (err) => {
  if (err) throw err

  console.log('已成功删除文件夹!')
})
```

如果 `myFolder` 中有其他文件和/或文件夹，如果未传递 `{ recursive: true }`，则会出现错误。

## 目录/文件重命名

使用 `fs` 模块，您可以重命名目录和文件。下面的代码片段显示了如何使用 `rename` 方法进行此操作。

```js
// 重命名目录
fs.rename(
  `${process.cwd()}/myFolder/secondFolder`,
  `${process.cwd()}/myFolder/newFolder`,
  (err) => {
    if (err) throw err

    console.log('目录重命名!')
  }
)

// 重命名文件
fs.rename(`${process.cwd()}/content.txt`, `${process.cwd()}/newFile.txt`, (err) => {
  if (err) throw err

  console.log('文件重命名!')
})
```

您可以看到 `rename` 方法包含三个参数：

- 第一个参数是现有的文件夹/文件
- 第二个参数是新名称
- 回调

因此，要重命名文件或目录，您需要传递当前文件/目录的名称和新名称。运行应用程序后，应更新目录/文件的名称。

> **需要注意的是**，如果新路径已经存在（例如，文件/文件夹的新名称），它将被覆盖。因此，请确保不要错误地覆盖现有文件/文件夹。

## 向文件中添加内容

使用文件系统模块，您还可以向现有文件添加新内容。`appendFile` 方法允许您这样做。

如果比较 `writeFile` 和 `appendFile` 这两种方法，我们可以发现它们是相似的。传递文件路径、内容和回调。

```js
fs.appendFile(filePath, '\nAll work and no play makes Jack a dull boy!', (err) => {
  if (err) throw err

  console.log('All work and no play makes Jack a dull boy!')
})
```

上面的代码片段演示了如何向现有文件添加新内容。如果运行应用程序并打开文件，您应该会看到其中的新内容。

## 读取文件的详细信息

每个文件都带有一组细节，我们可以使用 Node.js 来检查这些细节。

`stat` 方法检查给定路径文件的详细信息：

```js
const { stat } = require('fs')

stat('./file.txt', (err, stats) => {
  if (err) {
    console.error(err)
    return
  }

  // do something ...
})
```

同步方法 `statSync` 会阻塞线程，直到文件统计信息准备好：

```js
const { statSync } = require('fs')

try {
  const stats = statSync('./file.txt')
} catch (err) {
  console.error(err)
}
```

文件信息包含在 `stats` 变量中。包括：

- 如果文件是目录或文件，则使用 `stats.isFile()` 和 `stats.isDirectory()`
- 如果文件是符号链接，则使用 `stats.isSymbolicLink()`
- 文件大小（以字节为单位），使用 `stats.size`

还有其他高级方法，但在日常工作中大部分内容就是这些：

```js
const { stat } = require('fs')

stat('./file.txt', (err, stats) => {
  if (err) {
    console.error(err)
    return
  }

  stats.isFile() // true
  stats.isDirectory() // false
  stats.isSymbolicLink() // false
  stats.size //1024000 == 1MB
})
```

## 检查文件是否存在

`fs.exists` 已经废弃，建议使用 `fs.access`：

```js
const { access, constants } = require('fs')

const file = 'package.json'

// 检查当前目录中是否存在该文件
access(file, constants.F_OK, (err) => {
  console.log(`${file} ${err ? '不存在' : '存在'}`)
})
```

## stat() 与 fstat() 的区别

- `fs.stat` 接收的第一个参数是一个文件路径字符串
- `fs.fstat` 接收的是一个文件描述符

## readFile() 与 createReadStream() 的区别

- `readFile` 方法异步读取文件的全部内容，并存储在内存中，然后再传递给用户
- `createReadStream` 使用一个可读的流，逐块读取文件，而不是全部存储在内存中

与 `readFile` 相比，`createReadStream` 使用更少的内存和更快的速度来优化文件读取操作。如果文件相当大，用户不必等待很长时间直到读取整个内容，因为读取时会先向用户发送小块内容。

## watch() 与 watchFile() 的区别

二者主要用来监听文件变动：

- `fs.watch` 利用操作系统原生机制来监听，可能不适用网络文件系统
- `fs.watchFile` 则是定期检查文件状态变更，适用于网络文件系统，但是相比 `fs.watch` 有些慢，因为不是实时机制

## 截断文件内容

[`ftruncate`](https://nodejs.org/api/fs.html#fsftruncatefd-len-callback) 用于截断文件的内容。

语法：

```js
fs.ftruncate(fd, len, callback)
```

`close` 方法接受以下参数：

- `fd` — 文件 `fs.open()` 方法返回的文件描述符。
- `len` — 文件的长度，在此长度之后文件将被截断。
- `callback` — 回调函数，它不获取任何参数，除非给完成回调一个可能的异常。

截断文件内容示例：

```js
const { open, close, ftruncate } = require('node:fs')

function closeFd(fd) {
  close(fd, (err) => {
    if (err) throw err
  })
}

open('temp.txt', 'r+', (err, fd) => {
  if (err) throw err

  try {
    ftruncate(fd, 4, (err) => {
      closeFd(fd)
      if (err) throw err
    })
  } catch (err) {
    closeFd(fd)
    if (err) throw err
  }
})
```
