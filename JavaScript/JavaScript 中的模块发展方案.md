# JavaScript 中的模块发展方案

随着我们的应用越来越大，我们想要将其拆分成多个文件，即所谓的**模块（module）**。

在 ES6 之前，社区制定了许多种方法来将代码组织到模块中，使用特殊的库按需加载模块。

- [AMD](https://en.wikipedia.org/wiki/Asynchronous_module_definition)：最古老的模块系统之一，最初由 [require.js](http://requirejs.org/) 库实现。
- [CommonJS](http://wiki.commonjs.org/wiki/Modules/1.1)：为 Node.js 服务器创建的模块系统。
- CMD：代表的 [seajs](https://github.com/seajs/seajs) 库。
- [UMD](https://github.com/umdjs/umd)：另外一个模块系统，建议作为通用的模块系统，它与 AMD 和 CommonJS 都兼容。

## AMD

**[AMD](https://github.com/amdjs/amdjs-api/wiki/AMD)（Asynchronous Module Definition）即 "异步模块定义"**。它采用异步方式加载模块，模块的加载不影响它后面语句的运行。所有依赖这个模块的语句，都定义在一个回调函数中，等到加载完成之后，这个回调函数才会运行。

> **AMD** 提前执行，推崇依赖前置。

[RequireJS](https://github.com/requirejs/requirejs) 是 AMD 规范的最流行的实现。

它的出现为了解决两个问题：

- 实现 JavaScript 文件的异步加载，也就是模块是并行加载的，而不是通过等待加载完成来阻止执行。
- 管理模块之间的依赖性，便于代码的编写和维护。

**更多详细内容**：

- [JavaScript 模块化编程（二）：AMD 规范](http://www.ruanyifeng.com/blog/2012/10/asynchronous_module_definition.html)
- [JavaScript 模块化编程（三）：require.js 的用法](http://www.ruanyifeng.com/blog/2012/11/require_js.html)

## CMD

CMD 最为代表的 [seajs](https://github.com/seajs/seajs) 库。

这里简单带过，了解的不多，使用的也不多。

> CMD 延迟执行，推崇依赖就近，只有用户需要的时候才执行（性能好）

## UMD

[UMD](https://github.com/umdjs/umd)（Universal Module Definition，通用模块定义）模式，用于在任何地方工作的 JavaScript 模块。

如果你经常阅读源码，你会发现很多库都会使用 UMD：

```js
;(function (root, factory) {
  if (typeof define === 'function' && define.amd) {
    define([], function () {
      return factory(root)
    })
  } else if (typeof exports === 'object') {
    module.exports = factory(root)
  } else {
    root.myPlugin = factory(root)
  }
})(
  typeof global !== 'undefined'
    ? global
    : typeof window !== 'undefined'
    ? window
    : this,
  function (window) {
    'use strict'

    // do something ...
  }
)
```

它可以很好的兼容 AMD 和 CommonJS，无论在浏览器还是在其他 JS 运行环境。

## CommonJS

Node.js 应用由模块组成，其[模块系统](http://nodejs.org/docs/latest/api/modules.html)采用 CommonJS 规范。

CommonJS 是一种借助 `exports` 对象定义模块的方法，它定义了模块的内容。

```js
// someModule.js
exports.doSomething = function () {
  return 'foo'
}

// otherModule.js
var someModule = require('someModule')

exports.doSomethingElse = function () {
  return someModule.doSomething() + 'bar'
}
```

CommonJS 指定了一个全局方法 `require()` 来加载模块，获取依赖项，而 `exports` 用于导出模块内容的变量，以及一个模块标识符（描述与此模块相关的模块的位置），用于加载依赖项。

> **注意**：AMD 用于客户端（浏览器），CommonJS 主要用于服务端。

### CommonJS 模块的特点

- 所有代码都运行在模块作用域，不会污染全局作用域。
- 模块可以多次加载，但是只会在第一次加载时运行一次，然后运行结果就被缓存了，以后再加载，就直接读取缓存结果。要想让模块再次运行，必须清除缓存。
- 模块加载的顺序，按照其在代码中出现的顺序。

### CommonJS 原理与实现

浏览器不兼容 CommonJS 的根本原因，在于缺少四个 Node.js 环境的变量。

- `module`
- `exports`
- `require`
- `global`

只要能够提供这四个变量，浏览器就能加载 CommonJS 模块。

详细内容推荐阮一峰老师的[浏览器加载 CommonJS 模块的原理与实现](http://www.ruanyifeng.com/blog/2015/05/commonjs-in-browser.html)。

你也可以使用现有的 CommonJS 格式转换工具，[Browserify](http://browserify.org/) 是目前最常用的。

## ES Module

> ES6 Module 输出的是一个值的引用，编译时输出接口。ES6 模块不是对象，它对外接口只是一种静态定义，在代码静态解析阶段就会生成。

模块可以相互加载，并可以使用特殊的命令 `export` 和 `import` 来交换功能。

- `export` 命令用于规定模块的对外接口
- `import` 命令用于输入其他模块提供的功能

```js
export const name = 'lio'   // 命名导出
import { name } from 'file.js' // 命名导入

export default 'lio'       // 默认导出
import anyName from 'file.js' // 默认导入

export { name as newName }    // 重命名导出
import { newName } from 'file.js' // 命名导入

// 默认名称导出
export const name = 'lio'
export default 'lio'
import * as anyName from 'file.js' // 全部导入
// or
import anyName, { name } from 'file.js' // 默认 + 命名导入

// 导出列表，并重命名
export {
  name,
  age as anyAge
}
// 导入列表 + 重命名
import { name as newName, anyAge } from 'file.js'
```

模块路径必须是原始类型字符串，不能是函数调用

```js
import anyName from getModuleName() // Error, only from "string" is allowed
```

无法根据条件或者在运行时导入：

```js
if (...) {
  import ... // Error, not allowed!
}

{
  import ... // Error, we can't put import in any block
}
```

### 动态导入 `import()` 表达式

`import(module)` 表达式加载模块并返回一个 promise，该 promise 为一个包含其所有导出的模块对象。我们可以在代码中的任意位置调用这个表达式。

```js
// 📁 say.js
export function hi() {
  alert(`Hello`)
}

export function bye() {
  alert(`Bye`)
}

// index.js
let { hi, bye } = await import('./say.js')

hi()
bye()
```

> 动态导入在常规脚本中工作时，它们不需要 `script type="module"`。
>
> 尽管 `import()` 看起来像一个函数调用，但它只是一种特殊语法，只是恰好使用了括号（类似于 `super()`）。
>
> 因此，我们不能将 `import` 复制到一个变量中，或者对其使用 `call/apply`。因为它不是一个函数。

### 规范

下面将以 [Airbnb JavaScript 样式指南](https://github.com/airbnb/javascript)为参考，给出在使用 ES6 模块（`import/export`）的一些编写建议，其中包括 eslint（查找并修复 JavaScript 代码中的问题）的属性。

- 始终在非标准模块系统上使用模块（`import` / `export`）。

> 为什么？模块将会是未来的主流。

```js
// bad
const AirbnbStyleGuide = require('./AirbnbStyleGuide')
module.exports = AirbnbStyleGuide.es6

// ok
import AirbnbStyleGuide from './AirbnbStyleGuide'
export default AirbnbStyleGuide.es6

// best
import { es6 } from './AirbnbStyleGuide'
export default es6
```

- 不要使用通配符导入。

> 为什么？这样可以确保只有一个默认导出。

```js
// bad
import * as AirbnbStyleGuide from './AirbnbStyleGuide'

// good
import AirbnbStyleGuide from './AirbnbStyleGuide'
```

- 不要直接从导入中导出。

> 为什么？虽然 "一行" 很简洁，但有一个明确的导入方式和一个明确的导出方式使事情保持一致。

```js
// bad
export { es6 as default } from './AirbnbStyleGuide'

// good
import { es6 } from './AirbnbStyleGuide'
export default es6
```

- 仅从一个位置的路径导入。eslint：[`no-duplicate-imports`](https://eslint.org/docs/rules/no-duplicate-imports)

> 为什么？具有从同一路径导入的多行代码可能会使代码难以维护。

```js
// bad
import foo from 'foo'
import { named1, named2 } from 'foo'

// good
import foo, { named1, named2 } from 'foo'

// good
import foo, { named1, named2 } from 'foo'
```

- 不要导出可变绑定。eslint：[`import/no-mutable-exports`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/no-mutable-exports.md)

> 为什么？通常应该避免更改，特别是在导出可变绑定时。虽然在某些特殊情况下可能需要这种技术，但一般情况下，应该只导出常量引用。

```js
// bad
let foo = 3
export { foo }

// good
const foo = 3
export { foo }
```

- 具有单个导出功能的模块中，优先使用默认导出而不是命名导出。eslint：[`import/prefer-default-export`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/prefer-default-export.md)

> 为什么？鼓励更多只导出一件东西的文件，这对于可读性和可维护性而言更好。

```js
// bad
export function foo() {}

// good
export default function foo() {}
```

- 将所有 `import` 置于非 `import` 语句之上。eslint：[`import/first`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/first.md)

> 为什么？因为 `import` 被提升了，所以把它们都放在顶部，可以防止出现意外行为。

```js
// bad
import foo from 'foo'
foo.init()

import bar from 'bar'

// good
import foo from 'foo'
import bar from 'bar'

foo.init()
```

- 多行 `import` 应该像多行数组和对象字面量一样缩进。eslint：[`object-curly-newline`](https://eslint.org/docs/rules/object-curly-newline)

> 为什么？花括号遵循与风格指南中其他花括号块相同的缩进规则，尾随的逗号也一样。

```js
// bad
import { longNameA, longNameB, longNameC, longNameD, longNameE } from 'path'

// good
import { longNameA, longNameB, longNameC, longNameD, longNameE } from 'path'
```

- 在模块 import 语句中禁止 Webpack loader 语法。eslint: [`import/no-webpack-loader-syntax`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/no-webpack-loader-syntax.md)

> 为什么？因为在 Webpack 中使用了 `import` 语法，所以会将代码耦合到模块 loader 中。最好在 `webpack.config.js` 中使用 loader 语法。

```js
// bad
import fooSass from 'css!sass!foo.scss'
import barCss from 'style!css!bar.css'

// good
import fooSass from 'foo.scss'
import barCss from 'bar.css'
```

- 请勿将 JavaScript 文件扩展名包括在内。eslint：[`import/extensions`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/extensions.md)

> 为什么？包含扩展会将阻止重构，并对每个使用者中要导入的模块的实现细节进行不适当的硬编码。

```js
// bad
import foo from './foo.js'
import bar from './bar.jsx'
import baz from './baz/index.jsx'

// good
import foo from './foo'
import bar from './bar'
import baz from './baz'
```

推荐一篇关于 ES6 之前各种规范的模块化 [《JavaScript Module Pattern: In-Depth》](http://www.adequatelygood.com/2010/3/JavaScript-Module-Pattern-In-Depth)

也可以阅读阮一峰老师的 [JavaScript 模块化编程（一）：模块的写法](http://www.ruanyifeng.com/blog/2012/10/javascript_module.html)

## 总结

- AMD、CMD、CommonJS 和 ES6 Module 都是为了解决原始无模块化的痛点
- AMD — 代表 requirejs 库，提前执行，推崇依赖前置
- CMD — 代表 seajs 库，延迟执行，推崇依赖就近
- CommonJS — 模块输出的是一个值的拷贝，运行时加载，加载的是一个对象（`module.exports` 属性），该对象只有在脚本运行完才会生成
- ES6 Module — 模块输出的是一个值的引用，编译时输出接口，ES6 模块不是对象，它对外接口只是一种静态定义，在代码静态解析阶段就会生成

它们都是在 JS 推广模块定义过程的规范化产出。

## 更多资料

- [MDN Docs：Modules](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules)
- [现代 JS 教程：模块](https://zh.javascript.info/modules)
- [Relation between CommonJS, AMD and RequireJS?](https://stackoverflow.com/questions/16521471/relation-between-commonjs-amd-and-requirejs)
- [ES modules: A cartoon deep-dive](https://hacks.mozilla.org/2018/03/es-modules-a-cartoon-deep-dive/)
- [JavaScript modules](https://v8.dev/features/modules)
- [Understanding Modules and Import and Export Statements in JavaScript](https://www.digitalocean.com/community/tutorials/understanding-modules-and-import-and-export-statements-in-javascript)
- [More on importing and exporting](https://2ality.com/2014/09/es6-modules-final.html#more-on-importing-and-exporting)
- [前端模块管理器简介](http://www.ruanyifeng.com/blog/2014/09/package-management.html)
- [require() 源码解读](http://www.ruanyifeng.com/blog/2015/05/require.html)
- [JavaScript 模块的循环加载](http://www.ruanyifeng.com/blog/2015/11/circular-dependency.html)
- [JavaScript 模块化七日谈](https://huangxuan.me/js-module-7day/#/)
- [JavaScript 模块化编程简史（2009-2016）](https://yuguo.us/weblog/javascript-module-development-history/)
- [精读 js 模块化发展](https://zhuanlan.zhihu.com/p/26118022)
- [JavaScript 模块化发展](https://segmentfault.com/a/1190000015302578)
