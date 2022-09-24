# Vue Mixins

当你在 Vue 开发具有相似的组件时，你可能会一遍遍的复制粘贴相同的逻辑（`data`、`watch`、`computed` 等）。当然，你可能会想到将其编写为单个组件，并用 `props` 对其进行自定义。但是，如果有很多 `props` 很容易会造成混乱。

Vue 有一个很好的解决方案：[Mixins](https://v3.cn.vuejs.org/guide/mixins.html)，它是不同组件之间共享**可重用**代码的最简单方法之一。

一个 mixin 对象可以包含任意组件选项。当组件使用 mixin 对象时，所有 mixin 对象的选项将被合并到该组件本身的选项（有例外，往下看）。

> **注意**：Vue 3 不在建议在使用 Mixin（但未被移除），因为有更好的 Composition API 的支持，编写 Hooks 更有利于逻辑复用。

## 基本用法

mixin 对象可以使用**任何组件选项**（`data`、`watch`、`create` 和 `computed` 等），并且当组件使用 `mixin` 时，mixin 对象中的所有信息都将很好地混入组件中。然后，该组件可以访问 `mixin` 中的所有选项，就像您在组件本身中声明它一样。

定义一个 mixin 对象：

```js
// mixin.js
export default {
  data() {
    msg: 'Hello World'
  },
  created() {
    console.log(1)
  },
  methods: {
    displayMessage() {
      console.log(2)
    }
  }
}
```

将定义好的 mixin 通过通过组件的 `mixins` 选项合并到组件中：

```js
import mixin from './mixin.js'

new Vue({
  mixins: [mixin],
  created() {
    console.log(this.data)
    this.displayMessage()
  }
})
// 输出：
// 1
// {msg: 'Hello World'}
// 2
```

正如你所看到的，在使用 mixin 之后，该组件包含 mixin 中的所有数据，并且可以使用 `this` 进行访问。您也可以使用**变量**而不是单独的文件来定义 mixin。

> **注意**：在使用 Vue 生命周期钩子时，`mixins` 内的钩子将在组件的钩子之前被调用。

## 全局混入

上面的基本用法，主要是在单个组件内的选项合并，如果你需要每个组件都可以使用，可以在全局混入：

```js
Vue.mixin({
  created() {
    const myOption = this.$options.myOption
    if (myOption) {
      console.log(myOption)
    }
  }
})

new Vue({
  myOption: 'hello!'
})
// => "hello!"
```

> 请谨慎使用全局混入，因为它会影响每个单独创建的 Vue 实例（包括第三方组件）。大多数情况下，只应当应用于自定义选项，就像上面示例一样。推荐将其作为[插件](https://cn.vuejs.org/v2/guide/plugins.html)发布，以避免重复应用混入。

## 数据在各组件中互不影响

```js
// mixin.js
export default {
  data () {
    counter: 1
  }
}

// comp1.vue
import mixin from './mixin.js'
export default {
  mixins: [mixin],
  created() {
    this.counter++ // 2
  }
}

// comp2.vue
import mixin from './mixin.js'
export default {
  mixins: [mixin]
}
```

可以看到，两个组件都混入了对象，但两个组件间的操作不会互相影响。

## 命名冲突

当 Mixin 中存在与组件中的选项同名的 `data`、`method` 或任何组件选项时，组件及其 mixin 之间的命名**冲突**可能发生。如果发生这种情况，**组件本身**的属性将具有优先权。

例如，组件和 `mixin.js` 内同时存在 `data` 选项，都包含一个 `title` 属性，`this.title` 会返回组件中定义的值：

```js
// mixin.js
export default {
  data () {
    title: 'Mixin'
  }
}

import mixin from './mixin.js'
export default {
  mixins: [mixin],
  data () {
    title: 'Component'
  },
  created() {
    console.log(this.title) // "Component"
  }
}
```

如您所见，内部组件的数据优先于默认的 mixin 值。

## Mixins 与组件的优先级

我们在这里总结一下 Mixins 与组件的优先级：

- 组件中 `data` 定义的属性和 `methods` 中的方法会覆盖 mixins 中同名属性和方法
- 对于 `created`、`mounted` 等生命周期钩子，mixins 中生命周期钩子优先执行，再执行组件中生命周期钩子
- 对于相同的 `computed` 属性，组件的 `computed` 属性将覆盖 mixins 内的 `computed` 属性；而对于相同的 `watch` 监听，mixins 中的 `watch` 监听先执行。

## Mixins 和 Vuex 的区别

- vuex 用于状态管理，它定义的变量在每个组件中均可以使用和修改，在任一组件中修改此变量的值之后，其他组件中此变量的值也会随之修改。
- Mixins 可以定义共用的变量，在每个组件中使用，引入组件中之后，各个变量是相互独立的，值的修改在组件中不会相互影响。

## Mixins 和组件的区别

- 组件 — 在父组件中引入组件，相当于在父组件中给出一片独立的空间供子组件使用，然后根据 `props` 来传值，但本质上两者是相对独立的。
- Mixins — 在引入组件之后与组件中的对象和方法进行合并，相当于扩展了父组件的对象与方法，可以理解为形成了一个新的组件。
