# 显示构造函数模式 — Revealing-Constructor-Pattern

**显示构造函数模式** — 使用 `public` 和 `private` 方法创建多个脚本实例。

以下是一个简单的模板：

```js
var MyPlugin = (function () {
  'use strict'

  /**
   * 创建构造函数对象
   */
  var Constructor = function () {
    // 私有变量
    var publicAPIs = {}

    // 方法
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
     * 另外一个方法，用于插件初始化
     */
    publicAPIs.init = function (options) {
      // Code goes here...
    }

    // 返回公共 API
    return publicAPIs
  }

  // 返回构造函数
  return Constructor
})()
```

使用：

```js
// 实例化插件
var plugin = new MyPlugin()

// 使用公共方法
plugin.doSomething()
plugin.init()
```

您可以更改 `MyPlugin` 为您想用于插件的任何命名空间。构造函数以大写字母开头。
