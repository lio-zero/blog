# Vue 动态组件

Vue 动态组件可以非常方便地让代码更具可读性和适应性。它们可以将多个条件组件（使用 `v-if`、`v-else-if` 和`v-else` 切换的组件）简化为一行代码。

简而言之，它们允许您以最简单的方式在不同组件之间切换。

## 什么是动态组件？

动态组件是在运行时在组件之间动态切换的方法。

为了更好地理解，让我们看看一些示例。

- 浏览选项卡式组件而无需路由到新页面
- 在一个组件元素中管理不同类型的弹窗
- 根据用户是否登录显示不同的内容

所有这些都需要唯一的组件，但动态组件提供了一种快捷方式，而不必逐个包含所有组件。

使用动态组件有几个好处：

- 从开发人员的角度来看，它使您的代码更具可重用性。如果您只需要使用一个元素，而不是为所有内容添加一个 `v-if`，那么它的可伸缩性和可读性就更高。
- 从用户方面来说，使用动态组件可以使页面更具交互性，并节省页面负载，从而获得更快的用户体验。

> **这是双赢的。开发更容易，用户也会受益。**

现在我们知道了什么是动态组件以及它们为什么有用，让我们学习如何使用它们。

## 如何使用 Vue 动态组件？

使用动态组件非常简单。我们只需要知道两件事：

- `<component>` 组件
- `v-bind:is` 属性

`<component>` 组件可以根据数据的状态动态渲染不同的组件，而 `v-bind:is` 或 `:is`（简写）属性告诉 `<component>` 渲染哪个已注册组件。

`v-bind:is` 属性可以传递两种类型的值：

- 已注册组件名
- 实际导入的组件对象

使用也很简单，这里我们使用 `<script setup>` 语法糖。

只需要使用 `import` 语句导入我们想要使用的组件，声明一个变量来存放切换的组件，将 `component` 元素添加到模板中。

```html
<script setup>
  import { shallowRef } from 'vue'
  import ComponentA from '@/components/A.vue'
  import ComponentB from '@/components/B.vue'

  const comp = shallowRef(ComponentA)
</script>

<template>
  <div>
    <button @click="comp = ComponentA">Component A</button>
    <button @click="comp = ComponentB">Component B</button>
    <component :is="comp" />
  </div>
</template>
```

这里我们还添加了两个按钮来控制两个组件的切换。

[演示地址](https://sfc.vuejs.org/#eNqNUstugzAQ/JWVL2mlgO+IRIX+Qc++ELI0TvFDtiEHxL/XxojQtFS5wD5mxuNdD6TQOu07JBnJbW24dmDRdfrIJAAXWhkHA9hL1bbq9oENjNAYJWDnObsV5l35v0Tpirmf0iLo/okpF0w5YwKqVtI6/xUaDqsTX+7Sr0zmNLr0/nziUOi2cji5zc+8nwIfnjrnlIS3uuX114GRWfUuxchxSaDIaSQ8Sy9/0MtHeoDHVsbtTGcEaLRJo8+cLu7JnsQRJaLS6dUq6dcxBDCbG5aRDKZKqPmZhZyRi3PaZpTapg6DvNpUmU/qo9R00nGBKVqRnIy6WTRemJH9SoP6Yo8mMSjPaND8p/kA/aUbZEcmR3+Vae/hPW1tZzX47YFML+M5lXJLZfwGn0H5aA==)

Vue2 和 Vue3 写法区别不大，详细内容请查阅 [Dynamic Components](https://vuejs.org/guide/essentials/component-basics.html#dynamic-components) 和 [Built-in Special Elements](https://vuejs.org/api/built-in-special-elements.html#built-in-special-elements)。

您还可以为它添加更多其他的东西，例如：使用 [Vue 过渡](https://github.com/lio-zero/blog/blob/main/Vue/Vue%20%E8%BF%87%E6%B8%A1%E5%92%8C%E5%8A%A8%E7%94%BB.md)以使它无缝转换，[`keep-alive`](https://github.com/lio-zero/blog/blob/main/Vue/Vue%20keep-alive.md) 保存组件状态等。

> 但是即使在最简单的情况下，Vue 动态组件也可以使您的代码更具可维护性和适应性。
