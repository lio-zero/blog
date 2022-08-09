# 创建您的第一个 Vue 自定义指令

在 Vue 中，指令是直接编辑 DOM 的最佳方法之一。

例如 `v-if`、`v-show`、`v-bind` 等。如果您使用过 Vue，那么您一定对它们很熟悉。

正如您可能猜到的，[Vue 自定义指令](https://vuejs.org/guide/reusability/custom-directives.html#custom-directives)是 Vue 让我们为项目构建额外指令的方式。它们是在整个项目中添加独特且可重用功能的好方法。

Vue 自定义指令可以操作元素，也可以处理 DOM 中的响应性。

## 什么是自定义指令？

从本质上讲，自定义指令使您可以使项目适合您的需求。如果您使用 Vue 插件，您会注意到他们非常频繁地使用自定义指令。

例如，在 [vue-lazyload](https://github.com/hilongjw/vue-lazyload) 插件中，它使用指令 `v-lazy` 添加自定义功能，从而使图像加载更加有效。在这里使用指令是最好的情况，因为我们希望直接编辑 DOM。

> 您可能会问：“我不能注册 `computed` 和 `watch` 这样的组件选项吗？ ”

尽管组件选项对于抽象和代码重用很有用，但定制指令仍然是直接操作 DOM 元素的最佳方式之一。

## 指令钩子

与组件及其生命周期钩子一样，每个 Vue 指令都有自己的钩子可以触发。

Vue2 和 Vue3 中的指令钩子不同。

以下是 Vue2 指令钩子：

- `bind` — 当指令绑定到元素时调用一次
- `inserted` — 将绑定元素插入其父节点时
- `update` — 当元素更新时（但所有子元素尚未更新）
- `componentUpdated` — 子项也已更新后
- `unbind` — 当指令与元素解除绑定时调用一次

以下是 Vue3 指令钩子：

- `created` — 在应用元素的属性或事件侦听器之前调用。
- `beforeMount` — 与旧的 `bind` 钩子相同
- `mounted` — 与旧的 `inserted` 钩子相同
- `beforeUpdate` — 在元素本身更新之前调用（如生命周期钩子）
- `updated` — 与旧的 `componentUpdated` 钩子相同
- `beforeUnmount` — 在卸载元素之前调用（如生命周期钩子）
- `unmounted` — 与旧的 `unbind` 钩子相同

在实现这些钩子时，每个钩子都有一些可以接受的参数。

- `el` — 指令绑定到此元素；允许您修改它
- `binding` — 一个对象包含很多属性
- `vnode` — 虚拟节点
- `oldNode` — 先前的虚拟节点（仅在 `update` 钩子中可用）

> [Vue 文档中关于指令](https://vuejs.org/guide/reusability/custom-directives.html#directive-hooks)的一个重要注意事项是，您应该将这些参数（除 `el` 外）视为只读，并且永远不要修改它们。

### `binding` 对象

`binding` 对象包含几个属性，可以帮助您向钩子添加功能。

- `name` — 指令的名称（无 `v` 前缀）
- `value` — 传递给指令的值
- `oldValue` — 指令的前一个值（仅在 `update` 钩子中可用）
- `expression` — 绑定到字符串的表达式（例如 `v-my-directive = "3 * 3"`，`expression = "3 * 3"`）
- `arg` — 传递给字符串的任何参数（例如 `v-my-directive:blue`，`arg = blue`）
- `modifiers` — 所有作为对象传递的修饰符（例如 `v-my-directive.blue.link`，`modifiers = { blue：true, link：true }`）

## 添加自定义指令

在我们的 `main.js` 文件中或您定义 `Vue` 实例的任何位置，我们只需要使用 `Vue.directive` 方法即可。Vue 3 则是使用 [`app.directive`](https://vuejs.org/api/application.html#app-directive)。

现在，让我们创建一个 `v-font-size` 指令来操作组件的字体大小。

在 `main.js` 中，我们将添加一些代码来监听 `beforeMount` 和 `update` 钩子，并调整字体大小。

```js
// Vue 2
Vue.directive('font-size', {
  bind(el, binding) {
    el.style.fontSize = 24 + 'px'
  },
  updated(el, binding) {
    el.style.fontSize = 24 + 'px'
  }
})

// Vue 3
app.directive('font-size', {
  beforeMount(el, binding) {
    el.style.fontSize = 20 + 'px'
  },
  updated(el, binding) {
    el.style.fontSize = 20 + 'px'
  }
})
```

下面将使用 Vue3 钩子实现该指令，但是如果需要，您可以从上面将它们映射到 Vue2 钩子。

然后，在任何组件文件中，都可以使用前缀 `v-` 来访问它。

```html
<p v-font-size>使用 font-size 指令</p>
```

还有另一种定义 Vue 指令的方法。

```js
app.directive('font-size', (el, binding) => {
  el.style.fontSize = 24 + 'px'
})
```

如果传递的是一个函数而不是一个对象，那么它将在 `mounted` 和 `update` 钩子期间运行。

## 向指令传递参数

有几种方法可以增加对指令的控制。这可以通过传递附加值、参数或修饰符来实现。

### 传递值 — 用于响应性数据

传递数据最直观的方法就是给它一个值。这允许指令在值发生变化时进行响应性更新。这也提供了最灵活的控制，因为您可以接受广泛的值（任何字体大小）。

对于我们的示例，假设我们希望更好地控制元素中的字体大小。

```html
<p v-font-size="64">使用 font-size 指令</p>
<!-- 或者使用变量 -->
<p v-font-size="fontSize">使用 font-size 指令</p>
```

在指令内部，您需要更改方法以使用 `binding` 对象中的值。

```js
app.directive('font-size', (el, binding, vnode) => {
  el.style.fontSize = binding.value + 'px'
})
```

### 向指令发送参数

如果您真的不需要任何响应性，而只想提供一种为指令提供多个选项的方法。参数是做到这一点的好方法。

```js
app.directive('font-size', (el, binding, vnode) => {
  console.log(binding + ' ' + vnode)
  let defaultSize = 16
  switch (binding.arg) {
    case 'small':
      defaultSize = 12
      break
    case 'large':
      defaultSize = 32
      break
    default:
      defaultSize = 16
      break
  }
  el.style.fontSize = defaultSize + 'px'
})
```

然后在我们的模板中。

```html
<p v-font-size:small>小号 font-size 指令</p>
<p v-font-size:medium>中等 font-size 指令</p>
<p v-font-size:large>大号 font-size 指令</p>
```

### 使用修饰符

修饰符与参数类似，因为它们不适合响应性。但当与 `args` 结合使用时，您可以创建一个高度定制的系统。

这是因为可以对一个指令应用多个修饰符。

```js
app.directive('font-size', (el, binding, vnode) => {
  console.log(binding + ' ' + vnode)
  let defaultSize
  if (binding.modifiers.small) {
    defaultSize = 12
  } else if (binding.modifiers.large) {
    defaultSize = 32
  } else {
    defaultSize = 16
  }

  if (binding.modifiers.red) {
    el.style.color = '#ff0000'
  }

  el.style.fontSize = defaultSize + 'px'
})
```

然后我们可以在模板中使用它们：

```html
<p v-font-size.small.red>小号 font-size directive</p>
<p v-font-size.medium>中等 font-size directive</p>
<p v-font-size.large>大号 font-size 指令</p>
```

输出内容如下所示：

![v-font-size 指令](https://upload-images.jianshu.io/upload_images/18281896-cc1905a1b06090b9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
