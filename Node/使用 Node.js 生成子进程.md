# 使用 Node.js 生成子进程

Node.js 提供了一个 `child_process` 模块，该模块提供了生成子进程的能力。

导入该模块，并从中获取 `spawn`：

```js
const { spawn } = require('child_process')
```

然后你可以调用 `spawn()` 传递 2 个参数。

- 第一个参数是要运行的命令。
- 第二个参数是一个包含选项列表的数组。

这里有一个例子：

```js
spawn('ls', ['-lh', 'test'])
```

以上示例中，运行 `ls` 命令，有 2 个选项: `-lh` 和 `test`。这将生成命令 `ls -lh test`，它（假设 test 文件存在于您运行此命令的同一文件夹中）会生成有关文件的详细信息：

```bash
$ ls -lh test
# -rw-r--r-- 1 blog staff 6B Sep 25 09:57 test
```

调用 `spawn()` 方法的结果是 `ChildProcess` 类的一个实例，用于标识生成的子进程。

这里有一个稍微复杂一点的例子。我们观察 `test` 文件，当它发生变化时，我们在它上面运行 `ls -lh` 命令:

```js
'use strict'

const fs = require('fs')
const { spawn } = require('child_process')
const filename = 'test'

fs.watch(filename, () => {
  const ls = spawn('ls', ['-lh', filename])
})
```

还少了一样东西。我们必须将子进程的输出通过管道传输到主进程，否则我们将看不到它的任何输出。

我们通过调用子进程的 `stdout` 属性上的 `pipe()` 方法来实现:

```js
'use strict'

const fs = require('fs')
const { spawn } = require('child_process')
const filename = 'test'

fs.watch(filename, () => {
  const ls = spawn('ls', ['-lh', filename])
  ls.stdout.pipe(process.stdout)
})
```

## 更多资料

[在 Node.js 中使用 execa 运行命令](https://github.com/lio-zero/blog/blob/main/Node/%E5%9C%A8%20Node.js%20%E4%B8%AD%E4%BD%BF%E7%94%A8%20execa%20%E8%BF%90%E8%A1%8C%E5%91%BD%E4%BB%A4.md)
