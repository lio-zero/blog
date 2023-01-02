# Vue 无渲染组件

在 Vue.js 中，你可以使用无渲染组件来定义一个组件，这个组件并不会在 DOM 中渲染出任何内容。无渲染组件通常用于封装一些复杂的逻辑或提供一些公共的功能，但并不需要渲染任何 DOM 元素。

你可以使用虚拟节点 `VNode` 来定义一个无渲染组件。虚拟节点是一个用于表示 DOM 元素的 JavaScript 对象，但并不会真正地渲染出来。

举个例子，假设我们要定义一个名为 `RenderLess` 的无渲染组件：

```html
<!-- RenderLess.vue -->
<script>
  export default {
    render(createElement) {
      return createElement()
    }
  }
</script>
```

这个无渲染组件并不会在 DOM 中渲染出任何内容，但是你可以在模板中像普通组件一样使用它：

```html
<template>
  <div>
    <RenderLess></RenderLess>
  </div>
</template>
```

你还可以在无渲染组件中提供一些其他的功能，例如 `props`、`computed`、`methods` 等选项，就像普通组件一样。

例如，使用 `this.$scopedSlots.default` 访问组件的默认插槽，然后将一个对象作为参数传递给这个插槽，其中包含 `props` 传递过来的 `message` 属性。

```html
<!-- RenderLess.vue -->
<script>
  export default {
    props: ['message'],
    render() {
      return this.$scopedSlots.default({ message: this.message })
    }
  }
</script>
```

你可以像这样使用它：

```html
<template>
  <RenderLess message="Hello Vue!">
    <template v-slot="{ message }">
      <p>{{ message }}</p>
    </template>
  </RenderLess>
</template>

<script>
  import Renderless from './Renderless.vue'

  export default {
    components: {
      Renderless
    }
  }
</script>
```

这跟使用普通组件一样，只不过它不会渲染任何 DOM。

使用无渲染组件的好处在于，我们可以将逻辑与标签分离。

但需要注意，无渲染组件不能包含子组件，因为它们并不会在 DOM 中渲染出任何内容。

> 如果你对 Vue 插槽不是很了解，可以阅读 [Vue 插槽](https://github.com/lio-zero/blog/blob/main/Vue/Vue%20%E6%8F%92%E6%A7%BD.md) 了解插槽的用法。
