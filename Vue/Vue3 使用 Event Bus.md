# Vue3 使用 Event Bus

在 Vue2 中，创建 Event Bus 如下：

```js
export const bus = new Vue()
```

```js
bus.$on(...)
bus.$emit(...)
```

在 Vue3 中，Vue 不再是构造函数，而是 `Vue.createApp({})` 返回一个没有 `$on`、`$emit` 和 `$once` 方法的对象。

根据官方文档 [Vue 3 迁移指南](https://v3-migration.vuejs.org/breaking-changes/events-api.html#event-bus)所建议的，我们可以使用 [mitt](https://github.com/developit/mitt) 或 [tiny-emitter](https://github.com/scottcorgan/tiny-emitter) 库在组件之间调度事件。

这里我们使用 mitt，它的源码也很简单，👉 [地址](https://github.com/developit/mitt/blob/main/src/index.ts)。

## 安装

```js
pnpm i mitt
```

您也可以单独把代码拷贝一份到项目，代码量较少。

## 用法

与 Vue2 一样，封装为 `myBus.js`：

```js
import mitt from 'mitt'
export default mitt()
```

或者，你也可以定义为全局变量：

```js
import { createApp } from 'vue'
import App from './App.vue'
import mitt from 'mitt'

const app = createApp(App)
app.config.globalProperties.emitter = mitt()
```

然后，封装一个 hooks：

```js
// src/hooks/useEmitter.js
import { getCurrentInstance } from 'vue'

export default function useEmitter() {
  const internalInstance = getCurrentInstance()
  const emitter = internalInstance.appContext.config.globalProperties.emitter

  return emitter
}
```

当然，为了方便管理，你也可以在需要用到的地方单独引入 mitt。

## 示例

假设我们有一个 `sidebar` 和 `header`，其中包含一个关闭/打开侧栏的按钮，我们需要该按钮来切换侧边栏组件内的某些属性。

点击按钮发出 `toggle-sidebar` 带有一些 payload 的事件：

```html
<template>
  <button @click="toggleSidebar">toggle</button>
</template>
<script setup>
  import { ref } from 'vue'
  import useEmitter from '@/hooks/useEmitter'

  const sidebarOpen = ref(true)
  const emitter = useEmitter()

  const toggleSidebar = () => {
    sidebarOpen.value = !sidebarOpen.value
    emitter.emit('toggle-sidebar', sidebarOpen.value)
  }
</script>
```

在侧边栏中接收带有 payload 的事件：

```html
<template>
  <aside class="sidebar" :class="{'sidebar--toggled': !isOpen}">
    {{ isOpen }}
  </aside>
</template>
<script setup>
  import { ref, onMounted } from 'vue'
  import useEmitter from '@/hooks/useEmitter'

  const isOpen = ref(true)
  const emitter = useEmitter()

  onMounted(() => {
    emitter.on('toggle-sidebar', (isOpen) => {
      isOpen.value = isOpen
    })
  })
</script>
```

## 更多资料

[实现一个 Event Bus](https://github.com/lio-zero/blog/blob/main/%E6%89%8B%E5%86%99%E7%B3%BB%E5%88%97/%E5%AE%9E%E7%8E%B0%E4%B8%80%E4%B8%AA%20Event%20Bus.md)
