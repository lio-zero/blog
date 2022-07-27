# 构造函数模式 — Constructor Pattern

**构造函数模式** — 创建多个共享方法但包含唯一信息的脚本实例。

例如 `jQuery` 这样的 DOM 操作库。

以下是一个简单的模板：

```js
var MyPlugin = (function () {
  'use strict'

  /**
   * 创建构造函数对象
   */
  function Constructor(selector) {
    this.nodes = document.querySelectorAll(selector)
  }

  /**
   * 循环遍历每个元素
   */
  Constructor.prototype.forEach = function (callback) {
    for (var i = 0; i < this.nodes.length; i++) {
      callback(this.nodes[i], i)
    }
  }

  /**
   * 为每个元素添加一个类
   */
  Constructor.prototype.addClass = function (className) {
    this.forEach(function (node) {
      node.classList.add(className)
    })
  }

  // 静态构造方法
  /**
   * 获取页面的标签种类
   */
  Constructor.tagNameList = function () {
    return Array.from(document.getElementsByTagName('*')).map((v) => v.tagName)
  }

  // 返回构造函数
  return Constructor
})()
```

使用：

```js
MyPlugin.tagNameList() // [...]

// 创建构造函数的新实例
var headings = new MyPlugin('h2')

// 运行方法
headings.addClass('heading-big')
```

您可以更改 `MyPlugin` 为您想用于插件的任何命名空间。构造函数以大写字母开头。
