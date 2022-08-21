# Vue3 Suspense 组件

[`Suspense` 组件](https://vuejs.org/guide/built-ins/suspense.html#suspense)是 Vue3 新增的内置组件之一。它允许我们的应用程序在等待异步组件的同时渲染一些备用内容，让我们创造一个流畅的用户体验。

## 什么是 `Suspense` 组件？

`Suspense` 组件用于在等待某种异步组件解析时显示回退内容。

您可能会想，**我们什么时候会使用异步组件？**

老实说，比你想象的要多。每当我们希望我们的组件等待它获取数据（通常是在异步 API 调用中）时，我们可以使用 Vue3 Composition API 创建一个异步组件。

以下是一些异步组件有用的实例：

- 在加载页面之前显示加载动画
- 显示占位符内容
- 处理延迟加载的图像
- etc

以前，在 Vue2 中，我们必须使用条件（例如 `v-if` 或 `v-else`）来检查数据是否已加载并显示回退内容。

但是现在，Vue3 内置了 `Suspense`，所以我们不必担心数据加载时的跟踪和相应内容的渲染。

## 如何使用 Suspense 组件？

> 有两种类型的异步依赖可以等待 `<Suspense>`：
>
> - 带有 `async setup()` 钩子的组件。这包括使用 `<script setup>` 顶级 `await` 表达式的组件。
> - [异步组件](https://vuejs.org/guide/components/async.html)。
>
> 对于异步组件，你可以看看之前写过的一篇 [Vue3 中使用 defineAsyncComponent 延迟加载组件](https://github.com/lio-zero/blog/blob/master/Vue/Vue3%20%E4%B8%AD%E4%BD%BF%E7%94%A8%20defineAsyncComponent%20%E5%BB%B6%E8%BF%9F%E5%8A%A0%E8%BD%BD%E7%BB%84%E4%BB%B6.md)。

例如，我们有一个 `TodoInfo` 组件，其中使用了 `<script setup>`，该组件在完全渲染前异步加载一些数据。

```html
<template>
  <h1>{{ title }}</h1>
</template>

<script setup>
  const getTodoInfo = async () => {
    return await fetch('https://jsonplaceholder.typicode.com/todos/1')
      .then(response => response.json())
      .then(json => json)
  }

  var { title } = await getTodoInfo()
</script>
```

然后，我们有一个 `TodoList.vue` 组件，其中包含 `TodoInfo` 组件。

如果我们要在等待组件获取数据并解析时显示 `Loading...` 之类的内容，只需要三步：

- 将异步组件包装在 `<template #default>` 标签中
- 在异步组件旁边添加一个同级的 `<template #fallback>` 标签
- 将两个组件包装在 `<Suspense>` 组件中

使用插槽，`Suspense` 将渲染备用内容，直到默认内容准备就绪为止。然后，它会自动切换到显示我们的异步组件。

它看起来有点像这样。

```html
<Suspense>
  <template #default>
    <todo-info />
  </template>
  <template #fallback>
    <div>Loading...</div>
  </template>
</Suspense>
```

## 捕获组件错误

当我们使用异步组件时，我们还可以捕获错误并向用户显示一些错误消息。

在 Vue2 中，我们可以使用 [errorCaptured](https://vuejs.org/v2/api/#errorCaptured) 钩子，但是在 Vue3 中，它已重命名为 `onErrorCaptured`。

不管它被命名为什么，当任何子代组件的错误被捕获时，这个钩子就会运行。我们可以在 `Suspense` 中使用这个来渲染出错时的错误。

如果我们处理错误以显示错误消息，那么上面的组件就是这样的。

```html
<template>
  <div v-if="errMsg">{{ errMsg }}</div>
  <Suspense v-else>
    <template #default>
      <todo-info />
    </template>
    <template #fallback>
      <div>Loading...</div>
    </template>
  </Suspense>
</template>

<script setup>
  import { onErrorCaptured } from 'vue'

  const errMsg = ref(null)
  onErrorCaptured((e) => {
    errMsg.value = '出错了!'
    return true
  })
</script>
```

> 之前写过一篇相关的文章：[Vue 错误处理 — onErrorCaptured 钩子](https://github.com/lio-zero/blog/blob/master/Vue/Vue%20%E9%94%99%E8%AF%AF%E5%A4%84%E7%90%86%20%E2%80%94%20onErrorCaptured%20%E9%92%A9%E5%AD%90.md)。
