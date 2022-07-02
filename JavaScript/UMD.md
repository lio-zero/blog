# UMD

UMD（Universal Module Definition，通用模块定义） 提供对 JavaScript 模块捆绑器和加载器的支持，以及全局命名空间（和其他模式一样）。

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
})(globalThis, function (window) {
  'use strict'

  // do something ...
})
```

`globalThis` 是 ES11 新出的特性，如果需要支持低版本浏览器，可以将 `globalThis` 替换为如下：

```js
typeof global !== 'undefined'
  ? global
  : typeof window !== 'undefined'
  ? window
  : this
```

您可以更改 `myPlugin` 为您想用于插件的任何命名空间。

建议作为通用的模块系统，可以与任何模式一起使用，例如 AMD 和 CommonJS 都兼容。
