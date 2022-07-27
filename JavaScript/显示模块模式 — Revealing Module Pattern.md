# 显示模块模式 — Revealing-Module-Pattern

**显示模块模式**允许您将大部分变量和函数保留在全局范围之外，但将其中一些公开可用。

例如：像 `lodash` 这样的辅助库。

以下是一个简单的模板：

```js
var myPlugin = (function () {
  'use strict'

  // 私有变量
  var publicAPIs = {}

  /**
   * 私有方法
   */
  var somePrivateMethod = function () {
    // Code goes here...
  }

  /**
   * 公共方法
   */
  publicAPIs.doSomething = function () {
    somePrivateMethod()
    // Code goes here...
  }

  /**
   * 另一个公共方法，用于插件的初始化
   */
  publicAPIs.init = function (options) {
    // Code goes here...
  }

  // 最后，返回公共的 API 供调用
  return publicAPIs
})()
```

使用：

```js
myPlugin.doSomething()
myPlugin.init()
```

您可以更改 `myPlugin` 为您想用于插件的任何命名空间。
