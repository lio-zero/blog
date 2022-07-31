# Vue Context 参数 — Composition API

在 Composition API 中，我们没有 `this` 与 Options API 中相同的引用。

在 Options API 中，我们可以调用 `console.log(this)` 的任何选项并获得对组件本身的引用，让我们可以访问它的 `props`、`computed`、`data` 等。

Vue3 允许我们使用 Composition API，这些功能都位于 `setup` 函数内。这意味着 `setup` 是我们声明响应式数据、方法和计算属性的地方。

```js
import { ref } from 'vue'
export default {
  props: {
    lastName: String
  },
  setup() {
    // 没有 this，我们如何使用 props？
    const createdMethod = () => {
      console.log('created')
    }
    const name = ref('hello')

    createdMethod()

    return {
      createdMethod,
      name
    }
  }
}
```

`setup` 在我们的组件实例实际创建之前运行，并且由于我们的 `setup` 函数是我们实际上为组件定义所有内容的地方，因此不再使用 `this`。

## 如何访问组件属性？

Composition API 为我们提供了访问重要组件信息（如 `props` 和 `slot`）的替代方法。

`setup` 函数有参数让我们访问一些组件属性：`props` 和 `context`。

- `props` — 在我们的组件中包含定义的 `props`
- `context` — 是一个 JavaScript 对象，它公开了三个组件属性

这三个属性是：

- `context.attrs` – 传递给我们组件的**非 `props` 属性**
- `context.slots` – 具有我们所有**模板插槽渲染功能的对象**
- `context.emit` – 我们的组件**发出事件**的方法

让我们更深入地了解其中的每一个。

### context.attrs

假设我们有一个自定义组件，它接受一个名为 `value` 的 prop。

```
export default {
  props: {
    value: String,
  },
  setup(props, context) {
    console.log(context.attrs)
  }
}
```

然后在父组件中，我们向它传递几个属性。

```html
<template>
  <custom-component :value="value" test="hi" @close="close" />
</template>
```

将打印：

![context](https://upload-images.jianshu.io/upload_images/18281896-165fdc2b0234c6c7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

可以看到，它包含除了我们声明的 `props` 之外的所有内容。这包括事件监听器和 HTML 属性之类的东西。

这里的一个重要说明是 `attrs` 不是响应式的。这意味着如果我们想应用副作用来响应 `attrs` 值的变化，我们应该改用 `onUpdated` 生命周期钩子。

### context.slots

`context.slots`让我们可以访问每个 `slot` 的 `render` 方法。当我们编写自己的**自定义渲染函数**而不使用模板代码时，这很有用。

Vue 建议在大多数用例中使用模板，但如果你真的想使用 JavaScript 的全部功能，我们可以创建自己的渲染函数。

[Vue 文档](https://v3.vuejs.org/guide/render-function.html)中使用自定义 `render` 方法的一个很好的例子是，如果我们正在创建一个组件，该组件根据 `prop` 的值渲染具有不同级别标题的插槽值。

```
<template>
  <div>
    <h1 v-if="level == 1">
      <slot />
    </h1>
    <h2 v-if="level == 2">
      <slot />
    </h2>
    <h3 v-if="level == 3">
      <slot />
    </h3>
    <h4 v-if="level == 4">
      <slot />
    </h4>
    <h5 v-if="level == 5">
      <slot />
    </h5>
    <h6 v-if="level == 6">
      <slot />
    </h6>
  </div>
</template>

<script>
  export default {
      props: {
          level: Number
      }
  }
</script>
```

在这段代码中，我们对所有 6 个标题选项使用 `v-if` 和 `v-else-if` 条件。正如您所看到的，有很多重复的代码，而且看起来非常混乱。

相反，我们可以使用 `render` 函数以编程方式生成我们的标题。使用 Composition API `setup` 函数，它看起来像这样。

```js
import { h } from 'vue'
export default {
  props: { level: Number },
  setup(props, context) {
    return () => h('h' + props.level, {})
  }
}
```

但是，我们如何让我们的插槽进行渲染？

这就是 `context.slots` 发挥作用的地方。

通过让我们访问每个插槽的渲染函数，我们可以轻松地将插槽添加到渲染函数中。每个插槽都可以通过其名称访问，并且由于我们没有明确命名插槽，因此将其命名为 `default`。

```js
import { h } from 'vue'
export default {
  props: {
    level: Number
  },
  setup(props, context) {
    return () => h('h' + props.level, {}, context.slots.default())
  }
}
```

现在，如果我们使用像这样的简单父组件来运行它

```html
<template>
  <child-component :level="1"> Hello World </child-component>
</template>
```

这是我们完成的 DOM 的样子。

![image](https://upload-images.jianshu.io/upload_images/18281896-b928ef5bd27e89a1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

因此，您可能不会经常使用 `context.slots`，但是当您编写复杂的 JavaScript 渲染函数时，它是一个强大的功能。

> **注意**：根据 Vue 文档，`render` 函数的优先级高于根据 `template` 选项或挂载元素的 DOM 内 HTML 模板编译的渲染函数。

### context.emit

`context.emit` 将替换 `this.$emit` 作为我们从组件发出事件的方式。

这对于向父组件发送任何类型的事件（带或不带数据）非常有用。

假设我们有一个关闭按钮，它发出一个名为 `close` 的事件。

```html
<template>
  <div>
    <button @click="close">X</button>
  </div>
</template>

<script>
  export default {
    setup(props, context) {
      const close = () => {
        context.emit('close')
      }
      return {
        closeModal
      }
    }
  }
</script>
```

然后在我们的父组件中，我们可以使用 `v-on/@` 指令监听这个 `close` 事件。

```html
<modal-component @close="handleClose" />
```

## 我们在 `setup` 中无权访问什么

到目前为止，我们已经看到了 Composition API 如何使我们能够访问四个不同的属性：`props`、`attrs`、`slots` 和 `emit`。

但是由于 `setup` 在创建我们的组件实例之前运行，我们将无法访问这三个组件属性：

- `data`
- `computed`
- `methods`

例如，这些是我们在 `setup` 自身内部声明的属性，但我们没有内置方法来访问所有 `data` 属性的列表。

> 关于 Composition API 的问题，可以查看 [Composition API FAQ](https://vuejs.org/guide/extras/composition-api-faq.html)。
