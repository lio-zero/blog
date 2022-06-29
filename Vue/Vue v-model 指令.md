# Vue v-model 指令

`v-model` 是 Vue 应用程序中最常用的指令。它通常用于在表单元素上启用双向数据绑定，并与 `input`、`checkbox`、`select`、`textarea` 和 `radio` 一起使用。

在下面的例子中，`v-model` 应用于 `input` 元素，将 `someVal` 变量与 `input` 的原生值属性绑定在一起。

```html
<input v-model="someVal" />
```

`v-model` 本质上是一种语法糖，通过在内部为不同的输入元素使用不同的属性并抛出不同的事件。

上面的示例中，指令会监听原生的 `input` 事件，并在每次触发 `someVal` 时更新它。

因此，我们可以将上述代码重写为具有相同效果的事件和属性：

```html
<input :value="someVal" @input="someVal = $event.target.value" />
```

这就是 `v-model` 应用于常规输入的工作原理。

其他的表单元素如下：

- `text` 和 `textarea` 元素使用 `value` 属性和 `input` 事件
- `checkbox` 和 `radio` 使用 `checked` 属性和 `change` 事件
- `select` 字段将 `value` 作为 prop 并将 `change` 作为事件

## 自定义组件

我们就可以在每个会发出 `input` 事件并接受 `value` 值的组件上使用 `v-model`。

以下是一个 `myCounter` 组件：

```html
<template>
  <button @click="changeValue(modelValue - 1)">-</button>
  <span>{{ modelValue }}</span>
  <button @click="changeValue(modelValue + 1)">+</button>
</template>

<script setup>
  defineProps({
    modelValue: Number
  })
  const emit = defineEmits(['update:modelValue'])
  const changeValue = (newVal) => {
    console.log(newVal)
    emit('update:modelValue', newVal)
  }
</script>
```

由于每次更改 `update:modelValue` 事件时都会发出一个新值，并接受 `modelValue` 属性，因此我们可以安全地在此组件上使用 `v-model` 指令：

> **注意**：Vue 2.x 使用 `value` 属性和 `input` 事件。

```html
<myCounter v-model="count" />
<!-- 相当于 -->
<custom-input
  :modelValue="searchText"
  @update:modelValue="searchText = $event"
></custom-input>
```

## `v-model` 参数

`v-model` 还支持通过参数来修改默认名称：

```html
<template>
  <!-- v-model 参数 -->
  <custom-input v-model:title="pageTitle"></custom-input>
  <!-- 简写: -->
  <!-- <custom-input :title="pageTitle" @update:title="pageTitle = $event" /> -->

  <p>Title: {{ pageTitle }}</p>
</template>

<script setup lang="ts">
  import CustomInput from './CustomInput.vue'
  import { ref } from 'vue'

  const pageTitle = ref('Vue')
</script>
```

```html
<!-- CustomInput -->
<template>
  <input :value="title" @input="$emit('update:title', $event.target.value)" />
</template>

<script setup>
  defineProps({
    title: String
  })
</script>
```

## 多个 `v-model` 绑定

Vue3 允许我们在同一组件上绑定多个 `v-model`：

```html
<template>
  <custom-input v-model:title="pageTitle" v-model:content="pageContent" />
  <!-- 简写： -->
  <!-- <custom-input
    :title="pageTitle"
    @update:title="pageTitle = $event"
    :content="pageContent"
    @update:content="pageContent = $event"
  /> -->
  <p>Value: {{ pageTitle }}</p>
  <p>content: {{ pageContent }}</p>
</template>

<script setup>
  import CustomInput from './component/CustomInput.vue'
  import { ref } from 'vue'

  const pageTitle = ref('admin')
  const pageContent = ref('Element')
</script>
```

```html
<!-- CustomInput -->
<template>
  <input :value="title" @input="$emit('update:title', $event.target.value)" />
  <input
    :value="content"
    @input="$emit('update:content', $event.target.value)"
  />
</template>

<script setup>
  defineProps({
    title: String,
    content: String
  })
</script>
```

## 添加修饰符

`v-model` 支持的三个修饰符：

- `.lazy` — 监听 `change` 事件，而不是 `input`
- `.number` — 将有效输入字符串强制转换为数字
- `.trim` — 修剪输入

```html
<input type="text" v-model.trim.lazy="value" />
```

Vue3 还有一些其他的更改，如 `.sync` 修饰符被合并到了 `v-model` 中、自定义 `v-model` 修饰符。

## 更多资料

- [Form Input Bindings](https://vuejs.org/guide/essentials/forms.html)
- [Component Events](https://vuejs.org/guide/components/events.html#usage-with-v-model)
