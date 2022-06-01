# 使用 v-once 和 v-memo 指令来提升性能

优化渲染的一种极其简单的方法是仅重新渲染所需要的内容。

每当组件的数据发生更改时，该组件及其子组件将重新渲染，Vue 中的 `v-once` 和 `v-memo` 指令可以减少这种不必要的渲染。

## v-once

`v-once` 指令仅渲染元素/组件一次。初始渲染后，该元素及其所有子元素将被视为静态内容。

```html
<!-- 单个元素 -->
<span v-once>{{ msg }}</span>
<!-- 有子元素的元素 -->
<div v-once>
  <p>{{ msg }}</p>
</div>
<!-- 组件 -->
<my-component v-once :comment="msg"></my-component>
<!-- `v-for` 指令 -->
<ul>
  <li v-for="i in list" v-once>{{ i }}</li>
</ul>
```

当与 `v-if` 或 `v-show` 一起使用时，一旦我们的元素被渲染一次，`v-if` 或 `v-show` 将不再适用，这意味着如果它在第一次渲染时可见，它将始终可见。如果它是隐藏的，它将永远是隐藏的。

```html
<template>
  <p v-once v-if="show">{{ msg }}</p>
  <el-button @click="show = !show">切换</el-button>
</template>

<script setup>
  import { ref } from 'vue'

  const msg = ref('Hello')
  const show = ref(true)
</script>
```

以上 `p` 标签将始终可见。

## v-memo

[`v-memo`](https://vuejs.org/api/built-in-directives.html#v-memo) 是 Vue 3.2 新增的一个指令。它接受一个依赖数组，并且只有在数组中的一个值发生变化时才会重新渲染。

```html
<template>
  <p v-memo="[msg]">{{ msg }}</p>
  <el-button @click="msg = 'change msg'">切换</el-button>
</template>

<script setup>
  import { ref } from 'vue'

  const msg = ref('hello')
</script>
```

如果传入一个空的依赖项数组，它将与使用 `v-once` 相同，它永远不会重新渲染。

```html
<template>
  <p v-memo="[]">{{ msg }}</p>
  <!-- 等同于  -->
  <p v-once>{{ msg }}</p>

  <el-button @click="msg = 'change msg'">切换</el-button>
</template>

<script setup>
  import { ref } from 'vue'

  const msg = ref('hello')
</script>
```

**注意**，`v-memo` 在 `v-for` 循环中不起作用，所以如果我们想用 `v-for` 记忆一些东西，我们必须把它们放在同一个元素上。

## 最后

更多指令相关内容可以查看 [Built-in Directives](https://vuejs.org/api/built-in-directives.html)。
