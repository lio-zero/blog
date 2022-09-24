# Vue v-model 指令

`v-model` 是 Vue 应用中最常用的指令。它通常用于在表单元素上创建双向数据绑定，并与 `input`、`checkbox`、`select`、`textarea` 和 `radio` 表单元素一起使用。

在下面的例子中，`v-model` 应用于 `input` 元素，将 `someVal` 变量与 `input` 的原生值属性绑定在一起。

```html
<input v-model="someVal" />
```

`v-model` 本质上是一种语法糖，通过在内部为不同的 `input` 元素使用不同的属性并抛出不同的事件。

上面的示例中，指令会监听原生的 `input` 事件，并在每次触发 `someVal` 时更新它。

因此，我们可以将上述代码重写为具有相同效果的事件和属性：

```html
<input :value="someVal" @input="someVal = $event.target.value" />
```

不难看出，**单向绑定 + 事件**便可达到双向绑定的效果。

这就是 `v-model` 应用于常规输入的工作原理。

其他的表单元素如下：

- `input` 元素若是 `text / textarea` 类型，使用 `value` 属性和 `input` 事件。
- `input` 元素若是 `radio / checkbox` 类型，使用 `checked` 属性和 `change` 事件。
- `select` 元素使用 `value` 属性和 `change` 事件。

## 在组件上使用 `v-model`

要在组件上使用 `v-model` 指令实现双向绑定，我们可以在每个会发出 `update:modelValue` 事件并接受 `modelValue` 值的组件上使用 `v-model`。

> **注意**：Vue 2.x 使用 `value` 属性和 `input` 事件。

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
  <!-- 相同于 -->
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

Vue 3 允许我们在同一组件上绑定多个 `v-model`：

```html
<template>
  <custom-input v-model:title="pageTitle" v-model:content="pageContent" />
  <!-- 相同于 -->
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

- `.lazy` — 默认情况下，`v-model` 监听 `input` 事件，这意味着每次输入值都会同步更新数据。`.lazy` 修饰符将事件监听改为 `change` 事件，也就是失焦时同步更新数据。这减少了 Vue 实例同步次数，并且在某些情况下，可以显著提高性能。
- `.number` — 将有效输入字符串强制转换为数字。
- `.trim` — 修剪输入，想一下 JS 字符串的 `trim()` 方法。

```html
<input type="text" v-model.trim.lazy="value" />
```

你可以自由的链接多个修饰符以达到目的。

Vue 3 还有一些其他的更改，如 `.sync` 修饰符被合并到了 `v-model` 中、自定义 `v-model` 修饰符。

## 自定义 `v-model` 修饰符

Vue 允许我们自定义 `v-model` 修饰符。

以下是官网的一个[示例](https://vuejs.org/guide/components/events.html#usage-with-v-model)：

```html
<custom-input v-model.capitalize="text" />

<script setup>
  import { ref } from 'vue'
  const text = ref('')
</script>
```

```html
<input type="text" :value="modelValue" @input="emitValue" />

<script setup>
  const props = defineProps({
    modelValue: String,
    modelModifiers: { default: () => ({}) } // <- 关键
  })
  const emit = defineEmits(['update:modelValue'])
  const emitValue = (e) => {
    let value = e.target.value
    if (props.modelModifiers.capitalize) {
      value = value.charAt(0).toUpperCase() + value.slice(1)
    }
    emit('update:modelValue', value)
  }
</script>
```

组件的 `v-model` 上所添加的修饰符，可以通过 `modelModifiers` prop 在组件内访问到。

更多详细内容请看官网（实时更新）。

## 更多资料

- [Form Input Bindings](https://vuejs.org/guide/essentials/forms.html)
- [Component Events](https://vuejs.org/guide/components/events.html#usage-with-v-model)
