# Vue 依赖注入使用 Provide 和 Inject

使用 Provide 和 Inject 的 Vue 依赖注入对于构建 Vue 插件或避免 [prop Drilling](https://kentcdodds.com/blog/prop-drilling)（在层次结构中一路传递 prop，即使许多组件之间不需要 prop）提供帮助。

> 如果您使用过 React，那么您应该不陌生。

查看 [Composition API 文档](https://v3.vuejs.org/guide/component-provide-inject.html)，在 Vue3 中，使用 `provide` 和 `inject` 的依赖注入将更加常见。这主要是因为插件将不得不切换到使用这种模式，因为 Composition API 改变了这种引用（它不再让我们访问组件本身）。

在本文中，我们将介绍在 Vue3 中使用 `provide` 和 `inject`，以及如何使用它在组件层次结构中轻松分发内容。

## 什么是 Provide 和 Inject？

在 Vue 3 中，每个父级（或根 Vue 实例）都可以为其所有子级提供依赖关系。这包括深度嵌套的子级，无论组件层次结构有多深。

接着，我们可以把这个值注入任何一个子级。

![在深层组件结构中使用 Provide 和 Inject](https://upload-images.jianshu.io/upload_images/18281896-e968881c1407a949.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

基本上，我们所需要的只是依赖项的某个键，出于我们的目的，我们将使用一个简单的字符串。

然后，我们的 `provide` 方法将把键与某个值关联起来，我们的 `inject` 方法将使用相同的字符串检索该值。

来看一个简单示例：

```html
<!-- Root component -->
<script setup>
  import { provide } from 'vue'

  provide('open', false)
</script>
```

```html
<!-- DeepChild component -->
<script setup>
  import { inject } from 'vue'

  // 第二个可选参数是默认值（如果不存在）
  const open = inject('open', true) // false
</script>
```

可以看到，我们在根组件提供想要传递给后代组件的变量，然后它下面的每个组件都可以使用 `inject` 接受需要的变量。

### 全局依赖关系

如果我们想在全局范围内提供一些东西，我们可以使用 `app.provide` 在声明我们的 Vue 应用程序实例的任何地方使用。

```js
import { createApp } from 'vue'

const app = createApp({})

// 这在整个 Vue 应用程序下都可使用
app.provide('mode', 'dark')
```

这在构建 Vue3 插件时尤其有用。

## 使用 `ref` 来提供响应式数据

官网有这样一句话：

> `provide` 和 `inject` 绑定并不是可响应的。然而，如果你传入了一个可监听的对象，那么其对象的属性还是可响应的。

如果我们希望将响应式数据传递给子组件，这也非常方便。我们所要做的就是使用 `ref()` 向方法传递一个响应式属性。

```html
<!-- inside provider component -->
<script setup>
  import { provide, ref } from 'vue'

  const open = ref(false)

  function updateOpen() {
    open.value = !open.value
  }

  provide('open', {
    open,
    updateOpen
  })
</script>
```

```html
<!-- in injector component -->
<script setup>
  import { inject } from 'vue'

  const { open, updateOpen } = inject('open')
</script>

<template>
  <button @click="updateOpen">{{ open }}</button>
</template>
```

## 在 Option API 中使用提供和注入

到目前为止，我们已经看到了如何使用 `setup` 方法在 Composition API 中使用 `provide` 和 `inject`。但是，与 Vue3 的其他特性一样，同样的功能可以通过 Options API 来实现。

它们不是 `provide` 和 `inject` 方法，而是作为 `export default` 对象上的选项公开。

这些行为类似，我们只需要为我们想要提供的每个 prop 提供一个键和值。然后，无论我们想在哪里注入这些值，我们都可以在数组中列出特定属性的键。

```js
export default {
  // inside provider component
  provide: {
    open: false
  }
}
```

```js
export default {
  // in injector component
  inject: ['open'],
  created() {
    console.log(this.open) // false
  }
}
```

我们仍然可以注入响应式数据，但因为我们不使用 `ref`，我们可以在 Options API 中使用 `this`。

```js
export default {
  // inside provider component
  data() {
    return {
      status: false
    }
  },
  provide: {
    open: this.status
  }
}
```

```js
export default {
  // in injector component
  inject: ['open'],
  created() {
    console.log(this.open) // false
  }
}
```

## 什么时候使用 Provide/Inject？

`provide/inject` 是避免 **Prop 钻取**的一个好方法。回顾一下，当我们的根组件中有一个值，并且层次结构中只有一个子组件需要访问该值时，就需要进行 `provide/inject`。

如果我们只使用 `props`，我们需要不断地将这个 prop 逐级透传给所有中间组件，以便它到达我们层次结构的底部。

如果我们的代码库中发生某些变化，这会引入许多错误位置以及需要重构的位置。

`provide/inject` 通过只要求具有原始值的组件和需要该值的组件具有代码来修复此问题。这使得代码库的维护更加容易。

但是，仍有一些情况下 `props` 是更好的解决方案。

例如，如果我们需要确保我们的值遵循某种格式，那么 prop 验证非常有用。这可以包括字符串格式化、输入验证，甚至只是要求组件需要某些 prop。

这具体看项目场景而定。

另外，[vue-property-decorator](https://github.com/kaorun343/vue-property-decorator) 是一个 Vue 的属性装饰器，其中提供相同功能的 `@Provide` 和 `@Inject` 依赖注入装饰器。需要注意的是，它只支持到 Vue 2.6.12。
