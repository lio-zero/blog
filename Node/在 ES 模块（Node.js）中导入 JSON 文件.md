# 在 ES 模块（Node.js）中导入 JSON 文件

## 使用 `fs` 模块读取和解析 JSON 文件

Node.js 文档建议使用 `fs` 模块并自己完成读取文件和解析的工作。

```js
import { readFile } from 'fs/promises'
const json = JSON.parse(
  await readFile(new URL('./some-file.json', import.meta.url))
)
```

这种方法比起我们接下来用的第二种方法更容易理解。

## 利用 CommonJS `require` 方法加载 JSON 文件

我们还可以使用 `createRequire` 加载 JSON 文件。

`createRequire` 允许您构造 CommonJS `require` 方法，以便可以使用典型的 CommonJS 功能，例如在 Node.js 中读取 JSON ES 模块。

```js
import { createRequire } from 'module'
const require = createRequire(import.meta.url)
const data = require('./data.json')
```

前两种的区别：

- 二者的区别是 `require` 机制情况下，如果多个模块都加载了同一个 JSON 文件，那么其中一个改变了 JS 对象，其它跟着改变，这是由 Node 模块的缓存机制造成的，只有一个 JS 模块对象
- 第一种方式可以随意改变加载后的 JS 变量，而且各模块互不影响，因为他们都是独立的，是多个 JS 对象。

## import Assertions

JSON 模块已经存在于 Chrome 91，它看起来就像一个 ES Modules 风格的导入，只是你在最后设置了类型。

```js
import data from './data.json' assert { type: 'json' }

console.log(data)
```

当 Chrome 中有解决方案时，它很快就会在 Node.js 中发布。
