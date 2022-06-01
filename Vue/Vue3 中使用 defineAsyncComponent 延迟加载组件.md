# Vue3 中使用 defineAsyncComponent 延迟加载组件

使用 Vue3 的 `defineAsyncComponent` 特性可以让我们延迟加载组件。这意味着它们仅在需要时从服务器加载。

这是一个改善初始页面加载的好方法，因为我们的应用程序将以较小的块加载，而不必在页面加载时加载每个组件。

## 什么是 `defineAsyncComponent`

以下示例来自官网[异步组件](https://vuejs.org/guide/components/async.html)：

```js
import { defineAsyncComponent } from 'vue'

const AsyncComp = defineAsyncComponent(() => {
  return new Promise((resolve, reject) => {
    // ...load component from server
    resolve(/* loaded component */)
  })
})
// ... use `AsyncComp` like a normal component
```

我们还可以使用 `import` 从其他文件轻松添加 Vue 组件。

```js
import { defineAsyncComponent } from 'vue'

// 简单用法
const ArticleList = defineAsyncComponent(() =>
  import('@/components/ArticleList.vue')
)
```

这是使用 `defineAsyncComponent` 的最简单方法，但我们也可以传入一个完整的选项对象，该对象配置了几个更高级的参数。

```js
const AsyncPopup = defineAsyncComponent({
  loader: () => import('./ArticleList.vue'),
  // 加载异步组件时使用的组件
  loadingComponent: LoadingComponent,
  // 加载失败时使用的组件
  errorComponent: ErrorComponent
  // 在显示加载组件之前延迟。默认值：200ms。
  delay: 1000,
  // 超过给定时间，则会显示错误组件。默认值：Infinity。
  timeout: 3000
})
```

一般默认就已足够。

## 使用 `defineAsyncComponent` 延迟加载组件

我们简单的模拟请求文章数据的 `ArticleList.vue` 组件：

```html
// ArticleList.vue
<template>
  <div>{{ article }}</div>
</template>

<script setup>
  // API call
  const getArticleInfo = async () => {
    await new Promise(resolve => setTimeout(resolve, 1000))
    const article = {
      title: 'Vue3 中使用 defineAsyncComponent 延迟加载组件',
      author: 'lio'
    }
    return article
  }

  const article = await getArticleInfo()
</script>
```

我们使用 `defineAsyncComponent` 延迟加载 组件，并使用 `Suspense` 元素包装这个组件：

```html
<template>
  <button @click="show = true">Login</button>
  <Suspense v-if="show">
    <template #default>
      <ArticleList />
    </template>
    <template #fallback>
      <p>Loading...</p>
    </template>
  </Suspense>
</template>

<script setup>
  import { defineAsyncComponent } from 'vue'

  const ArticleList = defineAsyncComponent(() =>
    import('@/components/ArticleList.vue')
  )
  const show = ref(false)
</script>
```

**默认情况下，我们使用 `defineAsyncComponent` 定义的所有组件都是可挂起的。**

这意味着，如果组件的父链中存在 `Suspense`，它将被视为该 `Suspense` 的异步依赖项。我们的组件加载、错误、延迟和超时选项将被忽略，而将由 `Suspense` 处理。

最终效果如下：

![defineAsyncComponent](https://upload-images.jianshu.io/upload_images/18281896-37529d3041f24598.gif?imageMogr2/auto-orient/strip)

用户将看到 Loading...，然后在 1s 之后请求完数据后，展示完整的组件。

## 最后

当创建包含数十个组件的大型项目时，`defineAsyncComponent` 可能非常有用。当我们使用延迟加载组件时，我们可以加快页面加载时间，改善用户体验，并最终提高应用程序的保留率和转换率。
