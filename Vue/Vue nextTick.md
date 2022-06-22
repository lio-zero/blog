# Vue nextTick

Vue 在修改数据后，视图不会立即更新，而是在同一事件循环中的所有数据完成之后，再进行统一视图更新，即 Vue 中 DOM 更新是异步的。

而 [`nextTick`](https://vuejs.org/api/general.html#nexttick) 的作用正是在下次 DOM 更新循环结束之后执行延迟回调。常用于修改数据后获取更新后的 DOM。

以下是一个简单的示例：

```html
<template>
  <div ref="elem">{{ open }}</div>
</template>

<script setup>
  import { ref, nextTick, unref, onMounted } from 'vue'
  const elem = ref(null)
  const open = ref(true)

  onMounted(() => {
    open.value = false
    console.log(unref(elem).innerHTML) // true
    // DOM 还没有更新
    nextTick(() => {
      // DOM 更新了
      console.log(unref(elem).innerHTML) // false
    })
  })
</script>
```

尽管 MVVM 框架并不推荐访问 DOM，但有时确实免不了需要进行一些 DOM 操作，例如：使用第三方插件、视图更新后，基于新视图的进行操作等。而 `nextTick` 可以确保我们操作更新后的 DOM。
