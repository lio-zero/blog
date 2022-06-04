# 在 Vue2 与 Vue3 中构建相同的组件

**Vue2 和 Vue3 有什么不同？** 我们以一些简单的示例，来看看具体的变化。

你可以查看 [Vue3 文档](https://v3.vuejs.org/guide/introduction.html)，以了解更加详细的内容示例。

## 创建模板

Vue3 支持 **Fragments**，这意味着组件可以有多个根节点。

```html
<template>
  <div class="app"></div>
  <div class="popup"></div>
</template>
```

## 设置数据

主要的区别所在：`Options API` 与 `Composition API`

- `Options API` 将我们的代码分为不同的属性：`data`、`computed`、`methods` 等。
- `Composition API` 允许我们按函数而不是属性的类型对代码进行分组。

假设我们只有两个 `data` 属性：`username` 和 `password`。

Vue2 代码看起来像这样，我们只需在 `data` 属性中写上这两个值。

```js
export default {
  props: {},
  data() {
    return {
      username: '',
      password: ''
    }
  }
}
```

在 Vue3 中，使用一种新的 `setup()` 方法进行所有组件初始化的工作。

此外，为了使开发人员能够更好地控制响应性数据，我们可以直接访问 Vue 的 `reactive` API。

创建响应性数据包括三个步骤：

- 从 vue 导入 `reactive`
- 使用 `reactive` 方法声明我们的数据
- 让我们的 `setup` 方法返回响应式数据，以便我们的模板可以访问它

就代码而言，它看起来有点像这样。

```js
import { reactive } from 'vue'

export default {
  props: {},
  setup() {
    const state = reactive({
      username: '',
      password: ''
    })

    return { state }
  }
}
```

然后，在我们的模板中，像 `state.username` 和 `state.password` 那样访问它们。

## 在 Vue2 与 Vue3 中创建方法

**Vue2 Options API** 有一个单独的方法部分。在这里，我们可以定义所有的方法，并以我们想要的方式组织它们。

```js
export default {
  props: {},
  data() {},
  methods: {
    login() {}
  }
}
```

**Vue3 Composition API** 中的 `setup()` 方法也可以处理方法。它的工作原理与声明数据有些类似，我们必须先声明我们的方法，然后 `return` 它，以便组件的其他部分可以访问它。

```js
export default {
  props: {},
  setup() {
    // ...
    const login = () => {}
    return {
      login
    }
  }
}
```

## 生命周期钩子

在 Vue2 中，我们可以直接从组件选项访问[生命周期钩子](https://vuejs.org/guide/essentials/lifecycle.html)。以下使用到 `mounted` 钩子打印一些内容：

```js
export default {
  data() {},
  mounted() {
    console.log('component mounted')
  },
  ethods: {}
}
```

现在使用 **Vue3 Composition API**，几乎所有内容都在 `setup()` 方法内部。这包括 `mounted` 的生命周期钩子。

但是，默认情况下不包括生命周期钩子，因此我们必须导入 `onMounted` 方法作为在 `Vue3` 中调用的方法。

然后，在 `setup()` 方法中，我们可以通过传递函数来使用 `onMounted` 方法。

```js
import { reactive, onMounted } from 'vue'

export default {
  props: {},
  setup() {
    // ...
    onMounted(() => {
      console.log('component mounted')
    })
    // ...
  }
}
```

> 阅读：[Vue2 与 Vue3 生命周期变化](https://github.com/lio-zero/blog/blob/master/Vue/Vue2%20%E4%B8%8E%20Vue3%20%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F%E5%8F%98%E5%8C%96.md)

## 计算属性 — `computed`

让我们添加一个将用户名转换为小写字母的 `computed` 属性。

为了在 Vue2 中实现这一点，我们向 `options` 对象添加一个 `computed`。

```js
export default {
  // ..
  computed: {
    lowerCaseUsername() {
      return this.username.toLowerCase()
    }
  }
}
```

您可能也注意到了，在 Vue3 中使用一些方法，您需要从 vue 中导出它们，才能够使用。

> Vue3 的设计允许开发人员导入他们所使用的内容，并且在他们的项目中没有不必要的包。基本上，他们不希望开发人员必须包含他们从未使用过的东西，这在 Vue2 中正成为一个日益严重的问题。

因此，要在 Vue3 中使用 `computed` 属性，首先必须将 `computed` 导入到组件中。

然后，类似于我们之前创建 `reactive` 数据的方式，在添加一个属性，如下所示：

```js
import { reactive, onMounted, computed } from 'vue'

export default {
  props: {},
  setup() {
    const state = reactive({
      username: '',
      password: '',
      lowerCaseUsername: computed(() => state.username.toLowerCase())
    })

    // ...
  }
}
```

> 阅读：[Vue Computed — 计算属性](https://github.com/lio-zero/blog/blob/master/Vue/Vue%20Computed%20%E2%80%94%20%E8%AE%A1%E7%AE%97%E5%B1%9E%E6%80%A7.md)

## 访问 `props`

访问 `props` 在 Vue2 和 Vue3 之间的一个重要区别，**`this` 意味着完全不同。**

在 Vue2 中，`this` 几乎总是指组件，而不是特定的属性。虽然这让表面上的事情变得简单，但它使类型支持成为一种痛苦。

但是，我们可以很容易地访问 `props`，让我们添加一个简单的示例，例如在 `mounted` 钩子期间打印 `title`：

```js
export default {
  props: {
    title: {
      type: String,
      default: 'Vue2'
    }
  },
  mounted() {
    console.log('title: ' + this.title)
  }
}
```

但是在 Vue3 中，我们不再使用 `this` 来访问 `props`，发送事件和获取属性。相反，`setup()` 方法采用两个参数：

- `props` — 对组件 `props` 的不可变访问
- `context` — Vue3 公开的选定属性（`emit`、`slots` 和 `attrs`）

使用 `props` 参数，上面的代码如下所示。

```js
setup (props) {
  onMounted(() => {
    console.log('title: ' + props.title)
  })
}
```

## `emit` 事件

同样，在 Vue2 中**发出事件**非常简单，但是 Vue3 使您可以更好地控制如何访问属性/方法。

假设在我们的例子中，当我们按下**提交按钮**时，我们想向父组件发出一个 `login` 事件。

Vue2 代码只需调用 `this.$emit` 并传入我们的有效负载对象即可。

```js
login() {
  this.$emit('login', {
    username: this.username,
    password: this.password
  })
}
```

然而，在 Vue3 中，我们现在知道 `this` 不再意味着相同的事情，所以我们必须以不同的方式来做。

幸运的是，`context` 对象暴露了 `emit`，它提供了与 `this.$emit` 相同的东西。

我们所要做的就是将 `context` 作为第二个参数添加到 `setup` 方法中。我们将对 `context` 对象进行解构，使代码更加简洁。

然后，我们只需要调用 `emit` 发送事件即可。然后，像以前一样，`emit` 方法采用两个参数：

- 事件名称
- 与事件一起传递的有效 payload 对象

```js
setup(props, { emit }) {
  const login = () => {
    emit('login', {
      username: state.username,
      password: state.password
    })
  }
}
```

> 阅读：[在 Vue 中使用 $emit 自定义事件](https://github.com/lio-zero/blog/blob/master/Vue/%E5%9C%A8%20Vue%20%E4%B8%AD%E4%BD%BF%E7%94%A8%20%24emit%20%E8%87%AA%E5%AE%9A%E4%B9%89%E4%BA%8B%E4%BB%B6.md)

## 最后

Vue2 和 Vue3 中的所有概念都是相同的，但是我们访问属性的某些方式已经有所变化。

总的来说，我认为 Vue3 将帮助开发人员编写更有组织的代码，特别是在大型项目中。这主要是因为 **Composition API** 允许您按特定功能将代码分组在一起，甚至可以将功能提取到自己的文件中，然后根据需要将其导入组件中。
