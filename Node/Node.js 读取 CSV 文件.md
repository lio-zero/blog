# Node.js 读取 CSV 文件

许多不同的 npm 模块都允许你读取 CSV 文件。

它们中的大多数都是基于流的，例如 [`csv-parser`](https://github.com/mafintosh/csv-parser) 或 [`node-csv`](https://csv.js.org/)。

它们对于在生产系统中处理 CSV 非常有用。

当我不考虑性能时，我喜欢把事情简单化。例如，对于一次性解析 CSV，我必须合并我的后端系统。

为此，我使用 [`neat-csv`](https://github.com/sindresorhus/neat-csv) 包，它将 `csv-parser` 功能公开给一个简单的 `async/await` 接口。

使用 `pnpm i neat-csv` 安装它，并在你的应用程序中使用它:

```js
const neatCsv = require('neat-csv')
```

然后从文件系统加载 CSV 并调用 `neatCsv` 传递文件的内容：

```js
const fs = require('fs')

fs.readFile('./file.csv', async (err, data) => {
  if (err) {
    console.error(err)
    return
  }

  console.log(await neatCsv(data))
})
```

现在您可以开始对数据进行任何需要做的处理，这些数据被格式化为 JavaScript 对象数组。

## 更多资料

[将表格导出到 csv](https://github.com/lio-zero/blog/blob/main/JavaScript/%E5%B0%86%E8%A1%A8%E6%A0%BC%E5%AF%BC%E5%87%BA%E5%88%B0%20csv.md)
