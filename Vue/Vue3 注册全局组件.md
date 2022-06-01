# Vue3 注册全局组件

本文将介绍如何注册在整个 Vue 应用程序中使用的 Vue3 全局组件。这与我们在 Vue2 中声明它们的方式有些许不同，但是却很简单。

## 什么是 Vue 全局组件？

首先，我们必须了解 Vue3 全局组件是什么以及为什么要使用它。

通常，当我们想在 Vue 实例中包含一个组件时，我们会在本地注册它。通常看起来像这样。

```html
<script>
  import TkBadge from '../components/TkBadge.vue'

  export default {
    components: {
      TkBadge
    }
  }
</script>
```

但是，假设有一个组件，我们需要在 Vue 项目中**多次**使用它。在每个文件中本地注册此组件可能会很麻烦，尤其是在我们重构了项目或进行某些操作的情况下。

在这种情况下，全局注册组件可能会很有用，使它可以在我们的 Vue 实例的所有子组件中访问。换句话说，全局注册一个组件意味着我们不必将其导入每个文件中。

让我们看一下如何在 Vue2 中完成此操作以及现在如何在 Vue3 中进行操作。

## Vue2 全局组件

在 Vue2 中，无论我们在哪里创建 Vue 实例，我们都只需要调用 `Vue.component` 方法来注册全局组件。

此方法有两个参数：

- 我们全局组成部分的名称
- 组件本身

一个简单的实例：

```js
import Vue from 'vue'
import TkBadge from './components/TkBadge'
import App from './App.vue'

Vue.component('TkBadge', TkBadge) // 全局注册 - 我们可以在任何地方使用

new Vue({
  render: (h) => h(App)
}).$mount('#app')
```

现在，此 `TkBadge` 组件可以在此 Vue 实例的所有子组件中使用！

## Vue3 全局组件

在 Vue3 中，由于创建我们的 Vue 实例（使用 `createApp`）的工作方式略有不同，因此代码略有不同，但是它很容易理解。

首先必须创建应用程序，而不是从 Vue 对象中声明全局组件。然后，我们可以像以前一样运行相同的 `component()` 方法。

```js
import { createApp } from 'vue'
import TkBadge from './components/TkBadge'
import App from './App.vue'

const app = createApp(App)

app.component('TkBadge', TkBadge)

app.mount('#app')
```

如您所见，它们非常相似，但是 Vue 实例的初始化方式略有不同，使我们对语法进行了一些更改。

现在，你可以在根实例提供的任何组件中使用。

## 建议

- 重要的是要仔细考虑何时要使用全局组件还是本地组件。
- 如果默认情况下只是将所有内容都设为全局组件，则意味着即使不使用组件，该组件仍将包含在我们的构建中，增加页面加载时间。
- 正确使用全局组件可以成为非常强大的工具，并且随着 Vue3 中的新变化，在 Vue 项目中仍然很容易使用这些类型的组件。
