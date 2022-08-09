# Vue3 Teleport 组件

> [`<Teleport>`](https://v3.vuejs.org/guide/teleport.html)是一个内置组件，它允许我们将组件模板的一部分“传送”到存在于该组件 DOM 层次结构之外的 DOM 节点中。

## Teleport 的目的

我们首先要了解的是，何时使用、以及为什么使用 `Teleport`。

在处理更大的 Vue 项目时，以逻辑的方式组织代码库变得非常重要。然而，当处理某些类型的组件（如模态框、通知或工具提示）时，模板 HTML 的逻辑可能位于不同的文件中，而不是我们想要渲染元素的文件中。

事实上，很多时候，当这些元素与我们的 Vue 应用程序的 DOM 完全分开处理时，它们更容易管理。这一切都是因为处理嵌套组件的定位、`z-index` 和样式可能会因为处理其所有父组件的范围而变得棘手。

这就是 `Teleport` 功能派上用场的地方。我们可以在其逻辑所在的组件中编写模板代码，这意味着我们可以使用组件的 `data` 或 `props`。随后将其渲染到 Vue 应用程序的范围之外。

如果不使用 `Teleport`，我们将不得不担心事件传播，以便将逻辑从子组件传递到 DOM 树，但现在要简单得多。

让我们看一个例子。

## Teleport 示例

假设我们有一个子组件，我们想在其中触发弹出的通知。

首先，在 `</body>` 之前添加一个 `<div>`：

```html
<body>
  <div id="app"></div>
  <div id="notification"></div>
</body>
```

接下来，让我们开始创建触发要渲染通知的组件。

```html
<template>
  <teleport to="#notification">
    <div v-if="isOpen" class="notification">您干嘛呀！</div>
  </teleport>
</template>

<script setup>
  import { ref } from 'vue'

  const isOpen = ref(false)
  const closePopup: any = ref(null)
  const showNotification = () => {
    isOpen.value = true
    clearTimeout(closePopup.value)
    closePopup.value = setTimeout(() => {
      isOpen.value = false
    }, 2000)
  }
</script>

<style lang="scss" scoped>
  .notification {
    position: fixed;
    bottom: 20px;
    left: 20px;
    width: 300px;
    padding: 30px;
    background-color: plum;
  }
</style>
```

它的步骤很简单：

- 在您想要插入 DOM 的地方，预留一个标签，并提供一个 `id` 或 `class`，让 `teleport` 知道是哪个标签。
- 编写一段需要渲染的 HTML 结构，并使用 `teleport` 组件包裹住它。
- 在 `teleport` 标签上使用 `to` 属性，指向预留的标签。`to` 的值为提供标签的 `id` 或 `class`。

您可以指定多个 `Teleport` 用于不同的目的。详细内容查阅官网。
