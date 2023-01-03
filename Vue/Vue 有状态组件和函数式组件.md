# Vue 有状态组件和函数式组件

Vue.js 支持两种类型的组件：**有状态组件和函数式组件**。

## 有状态组件

有状态组件是完整的 Vue 组件，它具有自己的状态（也就是数据）和生命周期函数。有状态组件是以 Vue 类的形式定义的，并且通常使用模板来声明如何渲染组件。

以下是 Vue 中有状态组件的一个简单示例：

```html
<template>
  <div>
    {{ message }}
    <button @click="changeMessage">Change Message</button>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        message: 'Hello World!'
      }
    },
    methods: {
      changeMessage() {
        this.message = 'Hello Vue!'
      }
    }
  }
</script>
```

可以看到，它与普通的组件并无差异，它有自己的数据和方法，并使用模板声明如何渲染视图。

## 函数式组件

[函数式组件](https://cn.vuejs.org/guide/extras/render-function.html#functional-components)是没有状态的组件。它只是一个接收 `props` 选项并返回视图的纯函数。函数式组件的声明方式更简单，因此在某些情况下可以提高渲染性能。

以下是 Vue 中函数式组件的一个简单示例：

```html
<!-- MyComponent.vue -->
<script>
  export default {
    functional: true,
    props: ['message'],
    render(createElement, context) {
      return createElement('div', context.props.message)
    }
  }
</script>
```

我们添加 `functional: true` 表明当前组件是一个函数式组件。

示例中，它没有自己的数据和方法，而是使用纯函数的形式接收 `props` 并渲染视图。

你也可以用以下方式定义函数式组件：

```html
<!-- MyComponent.vue -->
<template functional>
  <h1>{{ props.message }}</h1>
</template>
```

然后，在其他组件中使用它：

```html
<template>
  <MyComponent v-bind="{ message: 'Hello Vue!' }" />
</template>

<script>
  import MyComponent from './MyComponent'
  export default {
    components: {
      MyComponent
    }
  }
</script>
```

## 最后

你可以根据组件的需求来选择使用哪种类型的组件。通常来说，如果组件需要拥有自己的状态，就使用有状态组件；如果组件只是简单地接收 `props` 并渲染视图，就使用函数式组件。
