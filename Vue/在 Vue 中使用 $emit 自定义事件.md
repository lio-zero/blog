# 在 Vue 中使用 $emit 自定义事件

Vue `$emit` 允许我们从子组件向其父组件发送自定义事件。

在标准 Vue 流中，这是触发某些事件或从子组件发送数据到父组件的最佳方式。

本文详细介绍 Vue `$emit`。

## Vue $emit 是如何工作的？

每个 Vue `$emit` 调用可以传递两个参数：

- **事件名称** — 这是我们可以在父组件中监听的事件名
- **有效 payload 对象** — 我们希望跟随事件一起传递的数据（可选）

```js
$emit('event-name', data)
```

在代码中使用 Vue `$emit` 有很多不同的方法：

- 使用 `$emit` 内联
- Options API — `this.$emit`
- Composition API — `context.emit`
- `setup script` 语法糖 `defineEmits` 方法

根据你的情况，每种方法都有自己的优缺点，所以让我们来看看每种方法的一个例子，你可以看到哪种方法适合你。

## 在 Vue 中使用 `$emit` 内联事件

在构建自己的自定义表单输入时，您可能希望从组件中发送数据。

```html
<!-- CustomInput.vue -->
<template>
  <input type="text" @change='$emit('customChange', $event.target.value)' />
</template>
```

以上存在一个问题，我们将无法监听像 `@change` 这样的标准输入事件。

假设我们的父组件是这样设置的，监听自定义事件 `custom-change` 并记录其值。

```html
<template>
  <CustomInput @custom-change="logChange" />
</template>

<script>
  import MyInput from './components/MyInput.vue'
  export default {
    components: {
      MyInput
    },
    methods: {
      logChange(event) {
        console.log(event)
      }
    }
  }
</script>
```

为了实现这一点，我们需要自定义文本输入来侦听本地输入事件，然后发出自己的事件。

要实际传递原始更改事件的值，我们需要将带有事件有效负载的自定义事件（在此例中为 `event.target.value`）作为第二个参数发送。

```html
<template>
  <input type="text" @change="$emit('customChange', $event.target.value)" />
</template>
```

现在，如果我们输入自定义输入，我们的父组件将正确地记录所有更改。

## 在 Options API 中以 `$emit()` 发送事件

在 Options API 中，可以使用 `this` 调用 `$emit` 方法，然后将 `e.target.value` 传递给它。

```html
<script>
  export default {
    methods: {
      customChange(e) {
        this.$emit('customChange', e.target.value)
      }
    }
  }
</script>
```

## 使用 `context.emit` 在 Composition API 中发送事件

在 Composition API 中，由于安装程序在创建组件之前运行，因此我们无法访问 `this`。

相反，我们可以使用 `setup` 函数的第二个参数 `context` 来访问 `emit` 方法。

> `context` 还可以获取 `slots` 和 `attrs`。

两种获取方法：

- 使用 `setup` 获取整个 `context` 对象
- 仅通过解构 `context` 获取 `emit`

我们可以调用与 Options API 完全相同的 `emit`：创建一个方法，调用 `emit`，并将参数传递给它！

```html
<script>
  export default {
    setup(props, { emit }) {
      const customChange = (e) => {
        emit('customChange', e.target.value)
      }

      return {
        customChange
      }
    }
  }
</script>
```

以上就是 Vue `emit()` 方法发送自定义事件的三种不同方法。

## setup 语法糖 `setup script`

可以通过 `defineEmits` 方法定义 emit 自定义事件。

```html
<script setup>
  const emit = defineEmits(['customChange'])
  const handleAPI = (e) => {
    emit('customChange', e.target.value)
  }
</script>
```

## 监听事件时使用 `kebab-case`

Vue 官方文档建议在事件名称中使用 **kebab-case**，即使是在脚本中。**如果您使用的是 Vue2，这一点至关重要**。

在 Vue2 中，事件名称没有自动将 **camelCase** 转换为 **kebab-case**，`v-on` 指令会自动将事件名称转换为小写，因此 **camelCase** 命名的事件不可能被侦听。

例如，如果我们发出一个名为 `myEvent` 的事件，则侦听 `my-event` **将不起作用**。

在 Vue3 中，事件名称（如 `props` 和 `components`）可以在不同情况下自动转换。与 `props` 类似，最好还是坚持每种编程语言的约定，**在脚本中使用 camelCase，在模板中使用 kebab-case**。
