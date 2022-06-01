# 制作您的第一个 Vue 插件

随着 Vue 的普及，社区创建的 Vue 插件变得越来越普遍和强大。

本文将带您创建您的第一个 Vue 插件。

## 如何设置插件

概括地说，我们的插件只是一个公开 `install()` 方法的 JavaScript 模块。该方法需要 2 个参数：

- Vue 构造函数
- `options` 对象

在我们的 Vue 项目中，在 `src/plugins` 文件夹下创建一个新的 `first-plugin.js` 文件。

其中，我们使用 `export default` 导出插件，并使用 `install(Vue, options)` 方法公开它。

```js
// first-plugin.js
export default {
  // 由 Vue.use(FirstPlugin) 调用
  install(Vue, options) {}
}
```

现在，让我们实际向该插件添加一些内容。

## 向插件添加功能

一种查看我们的插件运行情况的简单方法是创建一个全局 `mixin`，该全局 `mixin` 将包含在所有 Vue 实例中。使用 [`Vue.mixin`](https://vuejs.org/v2/guide/mixins.html#Global-Mixin) 函数可以做到这一点。

如果您不熟悉，这是 [`Vue mixins`](https://learnvue.co/2019/12/how-to-manage-mixins-in-vuejs/) 的基本概述。本质上，它们允许您注入到其他组件选项。它们是提取和重用组件之间通用功能的好方法。`Mixins` 还允许您的插件访问 Vue 生命周期钩子。

我们在 `Vue.mixin` 函数内声明其他组件选项。以下将在 `create` 生命周期钩子打印 Vue 实例：

```js
// first-plugin.js
export default {
  install(Vue, options) {
    // 创建一个 mixin
    Vue.mixin({
      created() {
        console.log(Vue)
      }
    })
  }
}
```

### 安装插件

如果您现在要运行项目，您会注意到什么都没有改变。那是因为我们还没有安装我们的插件。

在 `src/main.js` 文件中，即声明我们的 Vue 构造函数的文件中，我们只需要导入并安装插件文件即可。

```js
// main.js
import FirstPlugin from './plugins/first-plugin.js'

Vue.use(FirstPlugin)
```

**`Vue.use()` 的一大优点是可确保您的插件仅安装一次。**如果没有这一功能，而您又不小心将它添加了两次，它将减慢您的应用程序的运行速度，并可能使某些功能混乱。

现在，我们已经安装完插件。打开控制台，您将看到 Vue 实例的输出。

## 声明全局属性

**全局数据/方法**是一种向代码中添加广泛功能的有用方法。

假设我们希望应用程序的当前版本为全局属性。

```js
// first-plugin.js
install(Vue, options) {
  // 定义全局属性
  Vue.VERSION = 'v2.0.3'
}
```

> **注意**：不要在 Vue 实例内过度使用全局变量，它非常容易在全局作用域内过度拥挤，使用起来很痛苦。

## 定义实例属性

实例属性是将数据和方法添加到 Vue 项目的便捷方法。我更喜欢使用实例属性，以使我的全局作用域保持整洁并易于理解。

在此示例中，我刚刚创建了一个实例方法，该方法采用一个字符串并将其放置在 `<i>` 标签内。

```js
// first-plugin.js
// 定义实例方法
Vue.prototype.$italicHTML = function (text) {
  return `<i>${text}</i>`
}
```

> **注意**：`$` 不是必需的语法，这只是 Vue 用于实例属性以避免冲突的命名约定。

我们可以在任何组件内像这样使用它。

```html
<div v-html="$italicHTML(content)"></div>
```

## 全局过滤器

[Vue 过滤器](https://v3.cn.vuejs.org/guide/migration/filters.html#%E8%BF%87%E6%BB%A4%E5%99%A8)可以使文本转换变得非常容易。

> **注意**：Vue3 已移除过滤器。

同其他操作一样，我们要做的就是调用 Vue 构造函数方法 `Vue.filter()`，并提供一个全局可用的过滤器名称和如何处理文本内容的回调函数。

假设我们要使用过滤器来根据较长的文本生成预览代码片段。大致内容如下：

```js
// first-plugin.js
// 定义全局过滤器
Vue.filter('preview', (value) => {
  if (!value) {
    return ''
  }
  return value.substring(0, userOptions.cutoff) + '...'
})
```

## 添加自定义指令

[自定义指令](https://vuejs.org/v2/guide/custom-directive.html)是对特定元素进行 DOM 访问的好方法。

查看 [Vue 文档](https://vuejs.org/v2/guide/custom-directive.html)中的示例，让我们在插件内创建一个自定义指令，该指令自动将元素聚焦在页面上。

在 `install()` 方法内部，我们只需要使用 `Vue.directive()` 方法来声明我们的新指令。

```js
// first-plugin.js
// 添加自定义指令
Vue.directive('focus', {
  // 将绑定元素插入 DOM 时
  inserted(el) {
    // 聚焦元素
    el.focus()
  }
})
```

我们可以在任何组件内像这样使用它。这就是我们在页面加载时自动聚焦文本输入的方式。

```html
<input type="text" placeholder="text" v-focus />
```

## 合并选项对象

至此，我们还没有涉及 `install()` 方法的第二个参数。此方法使您的插件在不同情况下更加灵活。

为了使用 `options` 对象，我们首先向我们的插件传递一些选项供我们在插件内部选择。

使用 `options` 对象时，一种**最佳实践**是创建一些默认值。我们可以通过在插件文件中定义默认选项对象，然后使用 JavaScript 的[扩展语法](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax)将默认选项与参数 选项合并来实现这一点。

回顾我们前面过滤器的例子，假设我们要添加一个选项，该选项允许我们设置文本预览的截止点。看起来像这样：

```js
// first-plugin.js
const defaultOptions = {
  cutoff: 50
}

export default {
  // 由 Vue.use(FirstPlugin) 调用
  install(Vue, options) {
    // 将默认选项与 arg 选项合并
    let userOptions = { ...defaultOptions, ...options } // 其余插件代码
  }
}
```

现在，即使没有任何选项传递给插件，它仍将使用默认值运行。

如果我们确实想传递选项，那很简单。在 `src/main.js` 文件中，我们要做的就是在 `Vue.use()` 方法中添加第二个参数。此参数将是一个选项对象。

```js
// main.js
Vue.use(FirstPlugin, { cutoff: 100 })
```

因为我们将参数选项放在扩展语法的右侧，所以它会覆盖默认值。

## 最终的插件

```js
// first-plugin.js
const defaultOptions = {
  cutoff: 50
}

export default {
  install(Vue, options) {
    let userOptions = { ...defaultOptions, ...options }
    Vue.mixin({
      created() {
        console.log(Vue)
      }
    })
    Vue.VERSION = 'v2.0.3'
    Vue.prototype.$italicHTML = function (text) {
      return '<i>' + text + '</i>'
    }
    Vue.filter('preview', (value) => {
      if (!value) {
        return ''
      }
      return value.substring(0, userOptions.cutoff) + '...'
    })
    Vue.directive('focus', {
      inserted: function (el) {
        el.focus()
      }
    })
  }
}
```

就功能而言，此插件绝对遍地都是。但是，如果您继续进行下去，那么现在您已经熟悉了构建更高级的插件所需的大多数工具，方法和技术。

## 总结

本文，我们介绍了四种方法在插件内添加全局功能：

- `Vue.mixin`
- 实例属性和方法（`Vue.pertotype` 和 `Vue.pertotype.method`）
- `Vue.filter()` 过滤器（Vue3 已经将其移除）
- `Vue.directive()` 指令

## 更多资料

**[awesome-vue](https://github.com/vuejs/awesome-vue)** 库收集了与 Vue.js 相关的精彩内容的精选列表。
