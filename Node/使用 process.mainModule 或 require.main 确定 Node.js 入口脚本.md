# 使用 process.mainModule 或 require.main 确定 Node.js 入口文件

[ES2020](https://github.com/lio-zero/blog/blob/main/JavaScript/ES2020%EF%BC%88ES11%EF%BC%89.md) 的 `import.meta` 旨在解决访问模块元信息的问题，如文件当前元素是什么。

```html
<!-- index.html -->
<script src="foo.js"></script>
```

```js
// foo.js
const currentScript = document.currentScript
currentScript.src // 当前文件地址

// ESM 的获取方式 => 需要在 `script` 标签加上 `type="module"`
import.meta.url
```

这是我们在浏览器中执行此操作的方式，**但在 Node.js 中如何执行此操作？**

在 Node.js 中，每个模块和所需文件都包装在所谓的 [The module wrapper](https://nodejs.org/api/modules.html#modules_the_module_wrapper)（模块包装器）中。

```js
;(function (exports, require, module, __filename, __dirname) {
  // 模块代码实际上就写在这里
})
```

这就是 `require` 函数、`__filename` 和 `__dirname` 等便捷对象的来源。

在 Node.js 中没有 `currentScript`，而是一个入口文件，它可能引用了数千个其他模块。你现在**如何确定文件是否是入口文件**？

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

为了确定某个模块是否是入口文件，可以对照 `module` 对象进行检查。

```js
const isEntryScript = require.main === module
const isAlsoEntryScript = process.mainModule === module
```

**但是 `require.main` 和 `process.mainModule` 实际上是一样的吗？**

```js
console.log(require.main === process.mainModule) // true
```

运行后，你会发现它返回 `true`。**那么两者有什么区别呢？**

> 不同之处在于，如果主模块在运行时发生更改，`require.main` 可能仍然引用更改发生之前所需模块中的原始主模块。但通常，我们可以安全地假设这两个模块引用同一个模块。

**这是什么意思？**我决定稍微挖掘一下 Node.js 的核心代码。

`process.mainModule` 在 [node/lib/modules.js](https://github.com/nodejs/node/blob/59e48329d00cd91f6836cd91bcb8aca92acac1f6/lib/module.js#L503) 中定义：

```js
Module._load = function (request, parent, isMain) {
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

我没有进一步挖掘内部结构，但我们每天使用的 `require` 函数实际引用了 `process.mainModule`。这就是为什么它们实际上是一样的。**如果我们改变 `process.mainModule` 或 `require.main`，现在会发生什么？**

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

可以看到，如果我们在运行时将 `process.mainModule` 设置为其他值，`require.main` 仍然保留对初始主模块的引用。

> **注意**：`process.mainModule` 自 14.x 被废弃，但你仍然可以在 Node.js 内使用它。
>
> **扩展**：`import.meta.url` 需要在 ESM 环境中使用，这在 CJS 无法使用。如果你现在正在编写项目，需要在 CJS 和 ESM 中支持 `__dirname` 和 `import.meta.url`，可以看看 [antfu](https://github.com/antfu) 的 [Isomorphic `__dirname`](https://antfu.me/posts/isomorphic-dirname) 文章中给出的方法。但通常在构建需要同时支持 CJS 和 ESM 的包中，我们会使用一些工具同时生成它们，antfu 在 [Publish ESM and CJS in a single package](https://antfu.me/posts/publish-esm-and-cjs) 中介绍两种便捷的方式。另外，需要获取包根目录，可以看看 [Get Package Root](https://antfu.me/posts/get-package-root)。
