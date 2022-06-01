# Vue 动态组件

Vue 动态组件可以非常方便地让代码更具可读性和适应性。它们可以将多个条件组件（使用 `v-if`、`v-else-if`、`v-else` 切换的组件）简化为一行代码。

简而言之，它们允许您以最简单的方式在不同组件之间切换。

## 什么是动态组件？

动态组件是在运行时在组件之间动态切换的方法。

为了更好地理解，让我们看看一些示例。

- 浏览选项卡式组件而无需路由到新页面
- 在一个组件元素中管理不同类型的弹出窗口
- 根据用户是否登录显示不同的内容。

所有这些都需要唯一的组件，但动态组件提供了一种快捷方式，而不必逐个包含所有组件。

使用动态组件有几个好处。

- 从开发人员的角度来看，它使您的代码更具可重用性。如果您只需要使用一个元素，而不是为所有内容添加一个 `v-if`，那么它的可伸缩性和可读性就更高。
- 从用户方面来说，使用动态组件可以使页面更具交互性，并节省页面负载，从而获得更快的用户体验。

> **这是双赢的。开发更容易，用户也会受益。**

现在我们知道了什么是动态组件以及它们为什么有用，让我们学习如何使用它们。

## 如何使用 Vue 动态组件？

使用动态组件非常简单。我们只需要知道两件事：

- `<component>` 元素
- `v-bind:is` 属性

`<component>` 元素允许我们声明动态组件，并使用 `v-bind:is` 或 `:is`（简写）指向已注册的组件。

`v-bind:is` 属性可以传递两种类型的参数：

- 组件的名称
- 组件的选项对象

## 如何使用？

首先，我们必须使用 `import` 语句导入我们想要使用的组件。我们还必须在 `options` 方法中将它们声明为 Vue 属性的 `components`。

```html
<script>
  import ComponentA from '@/components/A.vue'
  import ComponentB from '@/components/B.vue'
  export default {
    components: {
      ComponentA,
      ComponentB
    }
  }
</script>
```

接下来，我们必须声明一个与当前组件对应的变量。看起来像这样。

```js
data () {
  return {
    comp: ComponentA
  }
}
```

最后，让我们将组件元素添加到模板中。这里添加了两个按钮来控制两个组件的切换。

```html
<template>
  <div>
    <button @click="comp = ComponentA">Component A</button>
    <button @click="comp = ComponentB">Component B</button>
    <component :is="component" />
  </div>
</template>
```

您还可以为它添加更多其他的东西，例如：使用 Vue 过渡以使它无缝转换，`keep-alive` 保存状态等。

> 但是即使在最简单的情况下，Vue 动态组件也可以使您的代码更具可维护性和适应性。
