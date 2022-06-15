# Node.js OS 模块

[OS 模块](http://nodejs.cn/api/os.html)提供有关计算机操作系统的信息。

获取有关计算机操作系统的信息：

```js
const os = require('os')

console.log('Platform: ' + os.platform()) // Platform: win32
console.log('Architecture: ' + os.arch()) // Architecture: x64
console.log('total memory : ' + os.totalmem() + ' bytes.') // total memory : 31194900400 bytes.
console.log('free memory : ' + os.freemem() + ' bytes.') // free memory : 30666943900 bytes.
```

- `os.platform()` 操作系统名
- `os.arch()` 操作系统 CPU 架构
- `os.totalmem()` 系统内存总量
- `os.freemem()` 操作系统空闲内存量
- ...

其他属性和方法查看文档即可。
