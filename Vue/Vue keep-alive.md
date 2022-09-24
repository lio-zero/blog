# Vue keep-alive

> **Tips**：本文示例仅限于 Vue 2。

当使用动态组件时，当您切换 [`:is` 指令](https://vuejs.org/v2/guide/components.html#Dynamic-Components)的值时，Vue 会重新创建组件的新实例。尽管它在大多数情况下很有用，但有时我们想要保存隐藏元素的状态。

这时 Vue 的 [`keep-alive`](https://vuejs.org/guide/built-ins/keep-alive.html#keepalive) 组件就派上用场了，它可以是提高速度并提供更好的用户体验的好方法。

## 什么是 `keep-alive`？

为了理解 `<keep-alive>`，您首先必须了解什么是动态组件。简而言之，它可以使用 `v-bind:is` 指令在不同组件之间切换。

最常见的示例是 **Tab 切换**，其中根据打开的选项卡，内容切换到不同的组件。

通常，当您在动态组件之间切换时，Vue 会为您的组件创建一个全新的实例。

然而，Vue `<keep-alive>` 是一个围绕动态组件的包装器元素。当组件处于非活动状态时，它会存储对组件的缓存引用。

这意味着 Vue 不必每次切换组件时都创建一个新实例。相反，当您返回时，它只使用缓存的引用。

`<keep-alive>` 是 Vue 的抽象元素，这意味着它既不渲染 DOM 元素，也不显示为组件。

> **Tips**：抽象元素使用 `abstract` 属性定义。

## `keep-alive` 应用场景

在大多数情况下，动态组件的内置功能是完美的。在某些情况下，您可能想要缓存状态，例如：

- 缓存表单上的用户输入，读取进度等。
- 你的组件会调用很多 API，而你只想调用一次
- 您的组件需要花一些时间来设置数据和计算属性，您希望在它们之间快速切换

## 基本使用

你可以在 [Vue keep-alive](https://codepen.io/lio-zero/pen/xxgeWzL) 进行简单的测试，已经给出基本模板。

假设我们有一个父选项卡组件，它有两个子组件 `About` 和`Contact`。

```html
<!-- About.vue -->
<template>
  <div>Hello Vue!</div>
</template>

<script>
  export default {
    mounted() {
      console.log('已挂载 About')
    }
  }
</script>

<!-- Contact.vue -->
<template>
  <div>
    <input type="text" placeholder="请输入内容..." />
    <input type="button" value="Send" />
  </div>
</template>

<script>
  export default {
    mounted() {
      console.log('已挂载 Contact')
    }
  }
</script>
```

父组件具有两个按钮，用于切换动态组件。

```html
<template>
  <div>
    <button v-for="tab in tabs" :key="tab" @click="component = tab">
      {{ tab }}
    </button>
    <component :is="component" />
  </div>
</template>

<script>
  import About from '@/components/About.vue'
  import Contact from '@/components/Contact.vue'
  export default {
    components: { About, Contact },
    data() {
      return {
        tabs: ['About', 'Contact'],
        component: 'About'
      }
    }
  }
</script>
```

现在，如果您运行您的应用，您应该会看到类似这样的内容。

![演示](https://upload-images.jianshu.io/upload_images/18281896-4b3b3966cac9152a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在组件之间切换时，您应注意：

- 每次切换选项卡时，来自 `mount()` 的消息都会打印在控制台中

![演示](https://upload-images.jianshu.io/upload_images/18281896-e9d99757a3f1ba1b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- 如果在 `Contact` 中输入内容，然后切换选项卡，在 `Contact` 选项卡输入的信息将不存在。

![演示](https://upload-images.jianshu.io/upload_images/18281896-9bb88642470e120f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这两个都是因为没有使用 `keep-alive`，Vue 会创建组件的新实例，因此所有生命周期钩子都会重新运行，并且您所做的任何输入都会丢失。

转到 `Tabs.vue` 组件，将动态组件包装在 `<keep-alive>` 组件中

```html
<keep-alive>
  <component :is="component" />
</keep-alive>
```

现在上面出现的问题都不存在：

- 来自 `mount()` 的消息应该由每个组件打印一次，并且只能打印一次
- 如果您在 `Contact` 选项卡上输入信息，则在切换选项卡返回时，该输入仍应存在

虽然这是使用 `keep-alive` 组件的简单用例，但它是说明为什么要使用它们的一个很好的例子。

## `include`、`exclude` 和 `max` 属性

`include` 和 `exclude` 允许组件有条件地缓存。二者都可以用逗号分隔字符串、正则表达式或一个数组来表示，是 2.1.0 新增的属性。

- `include` — 只有匹配的组件会被缓存。
- `exclude` — 任何匹配的组件都不会被缓存。

```html
<!-- 逗号分隔字符串 -->
<keep-alive include="Contact">
  <component :is="component"></component>
</keep-alive>

<!-- 正则表达式 -->
<keep-alive :include="/About|Contact/">
  <component :is="component"></component>
</keep-alive>

<!-- 数组 -->
<keep-alive :include="['About', 'Contact']">
  <component :is="component"></component>
</keep-alive>

<!-- 与 include 用法一致，但目的是任何匹配的组件都不会被缓存 -->
<keep-alive exclude="Contact">
  <component :is="component"></component>
</keep-alive>
```

匹配首先检查组件自身的 `name` 选项，如果 `name` 选项不可用，则匹配它的局部注册名称（父组件 `components` 选项的键值）。匿名组件不能被匹配。

`max` 表示最多可以缓存多少组件实例。一旦这个数字达到了，在新实例被创建之前，已缓存组件中最久没有被访问的实例会被销毁掉。是 2.5.0 新增的属性。

> `max` 的实现使用到了 [LRU 算法](https://en.wikipedia.org/wiki/Cache_replacement_policies#Least_recently_used_(LRU))，后面找个时间讲一讲。

```html
<keep-alive :max="2">
  <component :is="view"></component>
</keep-alive>
```

## `keep-alive` 生命周期钩子

为了帮助观察何时切换 `<keep-alive>` 的组件，我们有两个独特的钩子：

- `activated()` 在被 `keep-alive` 缓存的组件激活/进入时调用。
- `deactivated()` 在被 `keep-alive` 缓存的组件停用/离开时调用。

> **注意**：Vue 3 更名为 `onActivated` 和 `onDeactivated`。且这两个钩子在服务器端渲染（SSR）期间不被调用。

使用前面的示例使用这些钩子，在切换组件时将其输出到控制台。

```js
// About.vue
create() {
  console.log('About 数据初始化')
},
mounted() {
  console.log('About 已挂载')
},
activated () {
  console.log('About 已激活')
},
deactivated () {
  console.log('About 已停用')
}
```

现在，如果我们运行我们的应用并在选项卡之间切换，我们将看到数据初始化和挂载的消息仅打印一次，而激活/停用的消息则重复打印。

**注意**：当动态组件首次显示时，它触发 `create` 和 `mounted`，又触发 `activated`。因此，确保您不会两次计算某些逻辑很重要。

## 结合 router 缓存部分页面

在 router 中设置路由的元信息 `meta`

```js
export default new Router({
  routes: [
    {
      path: '/about',
      name: 'About',
      component: About,
      meta: {
        keepAlive: false // 不需要缓存
      }
    },
    {
      path: '/contact',
      name: 'Contact',
      component: Contact,
      meta: {
        keepAlive: true // 需要被缓存
      }
    }
  ]
})
```

使用 `$route.meta` 的 `keepAlive` 属性

```html
<keep-alive>
  <router-view v-if="$route.meta.keepAlive"></router-view>
</keep-alive>
<router-view v-if="!$route.meta.keepAlive"></router-view>
```

## `keep-alive` 的利弊

当然，使用 `<keep-alive>` 而不只是默认的动态组件是有利有弊。

- 优点：存储组件缓存，更快的组件
- 缺点：容易过度使用，通常情况下就足够了。

对于大多数情况，仅使用默认的动态组件而无需 `<keep-alive>` 是最佳解决方案。但是，如果您想轻松保存用户状态，则 `<keep-alive>` 组件是一种非常简单的方法。
