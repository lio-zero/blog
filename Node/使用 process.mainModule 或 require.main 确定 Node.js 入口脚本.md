# 使用 process.mainModule 或 require.main 确定 Node.js 入口脚本

[ES2020](https://github.com/lio-zero/blog/blob/main/JavaScript/ES2020%EF%BC%88ES11%EF%BC%89.md) 的 `import.meta` 旨在解决访问模块元信息的问题，如脚本当前元素是什么。

```html
<!-- index.html -->
<script src="foo.js"></script>
```

```js
// foo.js
const currentScript = document.currentScript
currentScript.src // 当前脚本地址
// ES 模块
import.meta.url
```

这是我们在浏览器中执行此操作的方式，但在 Node.js 中如何执行此操作？

在 Node.js 中，每个模块和所需文件都包装在所谓的 [The module wrapper](https://nodejs.org/api/modules.html#modules_the_module_wrapper)（模块包装器）中。

```js
(function(exports, require, module, __filename, __dirname) {
  // 模块代码实际上就写在这里
})
```

这就是 `require` 函数、`__filename` 和 `_ dirname` 等便捷对象的来源。

在 Node.js 中没有 `currentScript`，而是一个入口脚本，它可能引用了数千个其他模块。您现在如何确定脚本是否是入口脚本？

事实证明，有两种方法可以做到这一点：`require.main` 和 `process.mainModule`。

让我们看看这两个定义是什么。

```js
console.log(require.main)
console.log(process.mainModule)

/*
Module {
  id: '.',
  path: '',
  exports: {},
  parent: null,
  filename: '/www/wtf/foo.js',
  loaded: false,
  children: [
    Module {
      ...
    }
  ],
  paths:  [
    '/www/wtf/node_modules',
    '/www/node_modules',
    '/node_modules'
  ]
}
Module {
  id: '.',
  path: '',
  exports: {},
  parent: null,
  filename: '/www/wtf/foo.js',
  loaded: false,
  children: [
    Module {
      ...
    }
  ],
  paths:  [
    '/www/wtf/node_modules',
    '/www/node_modules',
    '/node_modules'
  ]
*/
```

可以看到，我们可以通过访问 `require.main.filename` 或 `process.mainModule.filename` 来获取入口模块的文件路径，这两个对象还包含更多有用的信息。

为了确定某个模块是否是入口脚本，可以对照 `module` 对象进行检查。

```js
const isEntryScript = require.main === module
const isAlsoEntryScript = process.mainModule === module
```

但是 `require.main` 和 `process.mainModule` 实际上是一样的吗？

```js
console.log(require.main === process.mainModule) // true
```

运行后，你会发现它返回 `true`。那么有什么区别呢？文件在这方面相对模糊。

> 不同之处在于，如果主模块在运行时发生更改，`require.main` 可能仍然引用更改发生之前所需的模块中的原始主模块。通常，可以安全地假设这两个模块引用同一个模块。

那是什么意思？我决定稍微挖掘一下 Node.js 的核心代码。

`process.mainModule` 在 [node/lib/modules.js](https://github.com/nodejs/node/blob/59e48329d00cd91f6836cd91bcb8aca92acac1f6/lib/module.js#L503) 中定义：

```js
Module._load = function(request, parent, isMain) {
  // ...

  if (isMain) {
    process.mainModule = module
    module.id = '.'
  }

  Module._cache[filename] = module

  tryModuleLoad(module, filename)

  return module.exports
}
```

`require.main` 在 [`node/lib/internals/modules.js`](https://github.com/nodejs/node/blob/9c6f6b0633ef4009cc04cfff5efb29c23ef5fc2b/lib/internal/module.js#L29) 中定义：

```js
function makeRequireFunction(mod) {
  // ...
  require.main = process.mainModule
  // ...
  return require
}
```

我没有进一步挖掘内部结构，但我们每天使用的 `require` 函数实际引用了 `process.mainModule`。这就是为什么它们实际上是一样的。如果我们改变 `process.mainModule` 或 `require.main`，现在会发生什么？

```js
// test.js
const bar = require('./foo')
console.log(process.mainModule)
console.log(require.main)

// foo.js
// 更改两个值
process.mainModule = 'A'
require.main = 'B'

/*
A
Module {
  id: '.',
  path: '',
  exports: {},
  parent: null,
  filename: '/www/wtf/foo.js',
  loaded: false,
  children:
   [
    Module {
      ..
    }
  ],
  paths:  [
    '/www/wtf/node_modules',
    '/www/node_modules',
    '/node_modules'
  ]
}
*/
```

如果我们在运行时将 `process.mainModule` 设置为其他值，`require.main` 仍然保留对初始主模块的引用。

> **注意**：`process.mainModule` 自 14.x 被废弃，但您还可以使用它。
