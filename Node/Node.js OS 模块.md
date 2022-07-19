# Node.js OS 模块

[OS 模块](https://nodejs.org/api/os.html)提供有关计算机操作系统的信息。

获取有关计算机操作系统的信息：

```js
const os = require('os')

console.log('Platform: ' + os.platform()) // Platform: win32
console.log('Architecture: ' + os.arch()) // Architecture: x64
console.log('total memory : ' + os.totalmem() + ' bytes.') // total memory : 31194900400 bytes.
console.log('free memory : ' + os.freemem() + ' bytes.') // free memory : 30666943900 bytes.
```

`os` 提供的主要方法：

- `os.platform()` 操作系统名，例如：`darwin`、`freebsd`、`linux`、`openbsd`、`win32` 等
- `os.arch()` 操作系统 CPU 架构
- `os.totalmem()` 返回操作系统中可用总内存的字节数
- `os.freemem()` 返回操作系统中可用空闲内存的字节数
- `os.arch()` 返回标识底层架构的字符串，如 `arm`、`x64` 和 `arm64`
- `os.cpus()` 返回有关系统上可用 CPU 的信息
- `os.userInfo()` 返回有关当前用户的信息
- `os.uptime()` 返回计算机自上次重新启动以来已运行的秒数
- `os.type()` 识别操作系统：Linux、macOS、Windows
- `os.tmpdir()` 返回分配的临时文件夹的路径
- `os.release()` 返回一个标识操作系统版本号的字符串
- `os.networkInterfaces()` 返回系统上可用网络接口的详细信息
- `os.hostname()` 返回主机名
- `os.homedir()` 返回当前用户主目录的路径

另外，

- `os.constants.signals` 告诉我们所有与处理进程信号相关的常量，如 `SIGHUP`、`SIGKILL` 等。
- `os.constants.errno` 设置错误报告的常量，如 `EADDRINUSE`、`EOVERFLOW` 等。

有关这些操作系统常量的内容，可以查阅 [OS constants](https://nodejs.org/api/os.html#os-constants)。
