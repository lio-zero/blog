# Vue 组件通信

本文将介绍 Vue 2 和 Vue 3 组件通信的各种方式。

## Vue 2 的组件通信

Vue 2.x 组件通信共有 12 种：

- `props`
- `$emit / v-on`
- `.sync`
- `v-model`
- `ref`
- `$children / $parent`
- `$attrs / $listeners`
- `provide / inject`
- `EventBus`
- `Vuex`
- `$root`
- `slot`

**父子组件通信**：

- `props`
- `$emit / v-on`
- `$attrs / $listeners`
- `ref`
- `.sync`
- `v-model`
- `$children / $parent`
- `provide / inject`（也可以，但不推荐）

**兄弟组件通信**：

- `EventBus`
- `Vuex`
- `$parent`

**跨层级组件通信**：

- `provide / inject`
- `EventBus`
- `Vuex`
- `$attrs / $listeners`
- `$root`

### `props`

`props` 是最常用的，用于父组件向子组件传送数据。

```html
<template>
  <ChildComponent :msg="msg" />
</template>
```

父组件通过 `props` 向下传递数据给子组件。子组件接受方式有两种：

- 数组
- 对象

使用对象可以规定接收的数据类型、默认值、验证等。

```js
// 数组
export default {
  props: ['msg']
}

// 对象
export default {
  props: {
    msg: {
      type: String,
      default: '默认值'
    }
  }
}
```

有几点需要注意：

- 子组件接收父组件的数据不能被直接修改，这样会报错。如果子组件使用到 `props` 进行修改的话可以借用 `computed`。
- 组件中的数据共有三种形式：`data`、`props`、`computed`。
- Vue 实例属性挂载顺序为 `props` —> `methods` —> `data` —> `computed` —> `watch`。不要出现同名属性的情况，否则数据将被覆盖/报错。

### `$emit/v-on`

`$emit/v-on` 通过事件形式传递数据。

子组件通过 `$emit` 派发事件的方式给父组件传递数据，触发父组件更新等操作

父组件：

```html
<template>
  <ChildComponent @sendMsg="getChildMsg" />
</template>

<script>
  export default {
    methods: {
      getChildMsg(msg) {
        console.log(msg)
      }
    }
  }
</script>
```

子组件：

```js
export default {
  data() {
    return {
      msg: 'Hello Vue'
    }
  },
  methods: {
    handleClick() {
      this.$emit('sendMsg', this.msg)
    }
  }
}
```

其中 `$emit` 的第二个参数附带子组件想要传递给父组件的数据。

### `.sync`

`.sync` 可以帮我们实现父组件向子组件传递数据的双向绑定。

子组件接收到数据后可以直接修改，并且会同时修改父组件的数据。

父组件：

```html
<template>
  <ChildComponent :open.sync="open" />
</template>

<script>
  import ChildComponent from './ChildComponent.vue'
  export default {
    data() {
      return {
        open: true
      }
    }
  }
</script>
```

子组件：

```js
export default {
  props: ['open'],
  computed: {
    currentPage: {
      get() {
        return this.page
      },
      set(newVal) {
        this.$emit('update:page', newVal)
      }
    }
  }
}
```

当我们在子组件里修改 `currentOpen` 时，父组件的 `open` 也会随之改变。

### `v-model`

`v-model` 与 `.sync` 类似，可以实现父组件向子组件传递数据的双向绑定。子组件通过 `$emit` 修改父组件的数据。

父组件：

```html
<template>
  <div>
    {{ value }}
    <ChildComponent v-model="value" />
  </div>
</template>

<script>
  import ChildComponent from './ChildComponent.vue'
  export default {
    data() {
      return {
        value: 'Hello Vue'
      }
    }
  }
</script>
```

子组件：

```html
<!-- 第一种：默认 prop 和 event -->
<template>
  <input :value="value" @input="handlerChange" />
</template>

<script>
  export default {
    props: ['value'],
    methods: {
      handlerChange(e) {
        this.$emit('input', e.target.value)
      }
    }
  }
</script>

<!-- 第二种：自定义 prop 和 event -->
<template>
  <input :value="message" @input="handlerChange" />
</template>

<script>
  export default {
    props: ['message'],
    model: {
      prop: 'message',
      event: 'change'
    },
    methods: {
      handlerChange(e) {
        this.$emit('change', e.target.value)
      }
    }
  }
</script>
```

默认情况下，一个组件上的 `v-model` 会把 `value` 用作 prop 且把 `input` 用作 event。也就是上面示例中的第一种方式。

而 `model` 选项允许一个自定义组件在使用 `v-model` 时自定义 prop 和 event。也就是上面示例中的第二种方式。

这种方式在 Vue 3 中得到简化。继续往下看。

### `ref`

通过在子组件上绑定 `ref`，从而获取子组件实例来调用组件的属性或方法。

父组件：

```html
<template>
  <ChildComponent ref="child" />
</template>

<script>
  import ChildComponent from './ChildComponent.vue'
  export default {
    mounted() {
      // 引用组件实例，获取组件的属性和方法
      const child = this.$refs.child
      console.log(child.name) // O.O
      child.handleMethod('调用了子组件的方法')
    }
  }
</script>
```

子组件：

```js
export default {
  data() {
    return {
      name: 'O.O'
    }
  },
  methods: {
    handleMethod(name) {
      console.log(name)
    }
  }
}
```

### `$parent / $children`

父组件使用 `$children` 访问子组件，子组件使用 `$parent` 访问父组件。

父组件：

```js
export default {
  data() {
    return {
      name: 'O.O'
    }
  },
  methods: {
    handleMethod() {
      return '父组件的方法'
    }
  },
  mounted() {
    // 获取第一个子组件中的属性
    this.$children[0].name // K.O
    // 调用第一个子组件的方法
    this.$children[0].handleMethod() // 第一个子组件的方法
  }
}
```

子组件：

```js
export default {
  data() {
    return {
      name: 'K.O'
    }
  },
  methods: {
    handleMethod() {
      return '第一个子组件的方法'
    }
  },
  mounted() {
    // 获取父组件中的属性
    this.$parent.name // O.O
    // 调用父组件的方法
    this.$parent.handleMethod() // 父组件的方法
  }
}
```

### `$attrs / $listeners`

多层嵌套组件传递数据时，如果只是传递数据，而不做中间处理的话就可以用这两个，比如父组件向孙子组件传递数据时。

- `$attrs` 包含父作用域里除 `class` 和 `style` 除外的非 `props` 属性集合。通过 `this.$attrs` 获取父作用域中所有符合条件的属性集合，然后还要继续传给子组件内部的其他组件，就可以通过 `v-bind="$attrs"`
- `$listeners` 包含了父作用域中不含 `.native` 修饰器的事件监听集合。如果还要继续传给子组件内部的其他组件，就可以通过 `v-on="$linteners"`。在创建更高层次的组件时非常有用。

父组件：

```html
<template>
  <ChildComponent :name="name" title="Hello Vue" />
</template>

<script>
  import ChildComponent from './ChildComponent.vue'
  export default {
    data() {
      return {
        name: 'O.O'
      }
    }
  }
</script>
```

子组件：

```html
<template>
  <!-- 继续传给孙子组件 -->
  <SubChild v-bind="$attrs" />
</template>

<script>
  import SubChild from './SubChild.vue'
  export default {
    props: ['name'], // 这里可以接收，也可以不接收
    mounted() {
      // 如果 props 接收了 name 就是 { title: 'Hello Vue' }，否则就是 { name: 'O.O', title: 'Hello Vue' }
      console.log(this.$attrs)
    }
  }
</script>
```

### `provide / inject`

`provide` 可以在祖先组件中指定我们想要提供给后代组件的数据或方法，而在任何后代组件中，我们都可以使用 `inject` 来接收 `provide` 提供的数据或方法。

父组件：

```js
export default {
  data() {
    msg: 'hi'
  },
  provide() {
    return {
      name: 'O.O',
      msg: this.msg,
      handlerMethod: this.handlerMethod
    }
  },
  methods: {
    handlerMethod() {
      console.log('这是注入的方法')
    }
  }
}

// 也可以使用这种方式，但无法使用组件的 this
export default {
  provide: {
    name: 'O.O'
  },
}
```

后代组件：

```js
export default {
  inject: ['name', 'msg', 'handlerMethod'],
  mounted() {
    console.log(this.msg) // hi
    this.handlerMethod()
  }
}
```

**注意**，provide`和`inject` 绑定并不是可响应的。然而，如果你传入了一个可监听的对象，那么其对象的属性还是可响应的。

### `EventBus`

使用 `EventBus`（**事件总线**）可以处理任何组件间的通信。

具体步骤是创建一个 Vue 实例，然后 `$on` 监听事件，`$emit` 来派发事件。

首先，创建并导出一个 Vue 实例：

```js
// src/eventBus.js
import Vue from 'vue'
export default new Vue()
```

祖先元素使用 `$on` 方法监听 `eventBus` 的事件：

```js
import bus from '@/eventbus'

export default {
  // ...
  mounted() {
    bus.$on('change-open', () => {
      this.open = !this.open
    })
  }
}
```

后代元素使用 `$emit` 触发 `eventBus` 的事件：

```js
import bus from '@/eventBus'

export default {
  // ...
  methods: {
    handleClick(e) {
      bus.$emit('change-open')
    }
  }
}
```

这种方式直接抽离成一个文件，可以在任何地方引入。

您也可以直接挂载到全局，或者注入到 Vue 的根对象上。

```js
// 挂载全局
import Vue from 'vue'
Vue.prototype.$bus = new Vue()

// 注入到 Vue 根对象上
new Vue({
  el: '#app',
  data: {
    Bus: new Vue()
  }
})
```

### `Vuex`

Vuex 可以在任何组件形式上实现通信，您可以阅读我之前写过的一篇 [Vuex](https://github.com/lio-zero/blog/blob/main/Vue/Vuex.md) 文章。

这里不过多介绍。

### `$root`

`$root` 为当前组件树的根组件实例，如果当前实例没有父组件，那么这个值就是它自己。

通过维护根实例上的 `data`，就可以实现组件间的数据共享。

```js
export default {
  mounted() {
    console.log(this.$root) // 直接访问到根组件
  }
}
```

### `slot`

`slot` 的作用域插槽可以将子组件的数据传递给父组件。

父组件：

```html
<Layout>
  <template v-slot:header="{ user }">
    {{ user }}
    <h1>我在父组件头部传递内容给子组件的插槽</h1>
  </template>
</Layout>
```

子组件：

```html
<template>
  <header>
    <slot name="header" :user="user"></slot>
  </header>
</template>
<script>
  export default {
    data() {
      return {
        user: { name: 'O.O' }
      }
    }
  }
</script>
```

详细内容可以阅读 [Vue 插槽](https://github.com/lio-zero/blog/blob/main/Vue/Vue%20%E6%8F%92%E6%A7%BD.md)。

## Vue 3 的组件通信

Vue 3 基于新的响应式系统重新实现，它的一些新的功能、API 也有所改变。

以下介绍的内容为 Vue 3.2 以上版本。Vue 3 版本迭代很快，可能有一些 API 略有不同，详细请查阅官网。

### `props` 属性

父组件与子组件传值，通过 `props`：

```html
<template>
  <Child name="Vue 3" />
</template>
```

子组件：

```html
<template>
  <div>{{ name }}</div>
</template>

<script setup>
  const props = defineProps({
    name: String
  })

  console.log(props.name) // Vue 3
</script>
```

Vue 3 的示例中，我们都使用 `setup` 语法糖。

### `$emit`

`$emit` 我在 [Vue $emit 自定义事件](https://github.com/lio-zero/blog/blob/main/Vue/%E5%9C%A8%20Vue%20%E4%B8%AD%E4%BD%BF%E7%94%A8%20%24emit%20%E8%87%AA%E5%AE%9A%E4%B9%89%E4%BA%8B%E4%BB%B6.md) 有介绍到，这里我们简单的写一个示例。

子组件向父组件传递数据：`$on` 和 `$emit`。

父组件使用 `$on`（简写 `@`）绑定一个自定义事件（这里是 `@custom-change`）接受子组件传递过来的数据：

```html
<template>
  <MyInput @custom-change="handleChange" />
</template>

<script setup>
  import MyInput from './MyInput.vue'
  const handleChange = (value) => {
    console.log('Child msg: ', value)
  }
</script>
```

子组件通过 `$emit` 方法传递数据给父组件：

```html
<!-- MyInput.vue -->
<template>
  <input type="text" @change="handleEmit" />
</template>

<script setup>
  const emit = defineEmits(['customChange'])

  const handleEmit = (e) => {
    emit('customChange', e.target.value)
  }
</script>
```

### `attrs`

`attrs` 包含父作用域里除 `class` 和 `style` 除外的非 `props` 属性集合。

父组件：

```html
<template>
  <Child :firstName="firstName" :lastName="lastName" name="D.O" />
</template>

<script setup>
  import Child from './child.vue'
  import { ref } from 'vue'

  const firstName = ref('Hello')
  const lastName = ref('Vue')
</script>
```

子组件：

```html
<script setup>
  import { useAttrs } from 'vue'
  defineProps({
    firstName: String
  })
  const attrs = useAttrs()
  console.log(attrs) // { lastName: "Vue", name: "D.O" }
</script>
```

这里 `firstName` 已经被 props 接收，所以 `attrs` 只有 `lastName` 和 `name`。

### `v-model` 指令

关于 `v-model`，我在 [Vue v-model 指令](https://github.com/lio-zero/blog/blob/main/Vue/Vue%20v-model%20%E6%8C%87%E4%BB%A4.md)做了详细介绍，下面简单带过。

我们可以在组件上使用 `v-model` 指令实现双向绑定。

父组件：

```html
<template>
  <Child v-model:first="firstName" v-model:last="lastName" />
  <div>{{ firstName }} {{ lastName }}</div>
</template>

<script setup>
  import Child from './child.vue'
  import { ref } from 'vue'

  const firstName = ref('Hello')
  const lastName = ref('Vue2')
</script>
```

子组件：

```html
<template>
  <button @click="handlerClick">按钮</button>
</template>

<script setup>
  const emit = defineEmits(['update:first', 'update:last'])

  const handlerClick = () => {
    emit('update:first', 'Hello')
    emit('update:last', 'Vue3')
  }
</script>
```

### `defineExpose / ref`

通过 `defineExpose` 暴露子组件的属性或方法，然后在父组件引用子组件的位置绑定 `ref`，获取子组件的属性或调用子组件方法。

父组件：

```html
<template>
  <Child ref="child" />
  <button @click="handlerClick">按钮</button>
</template>

<script setup>
  import Child from './child.vue'
  import { ref } from 'vue'

  const child = ref(null)
  const handlerClick = () => {
    // 获取子组件对外暴露的属性和方法
    console.log(child.value.name) // O.O
    child.value.sayHi() // Hello Vue
  }
</script>
```

子组件：

```html
<script setup>
  defineExpose({
    name: 'O.O',
    sayHi() {
      console.log('Hello Vue')
    }
  })
</script>
```

### `provide/inject`

`provide / inject` 为依赖注入，可以穿透多层组件，实现数据从父组件传递到子组件。

- `provide` — 可以让我们指定想要提供给后代组件的数据
- `inject` — 在任何后代组件中接收想要添加在这个组件上的数据，不管组件嵌套多深都可以拿到

父组件：

```html
<template>
  <div>provide 和 reject</div>
  <Child />
</template>

<script setup>
  import Child from './child.vue'
  import { provide } from 'vue'

  provide('name', 'O.O')
</script>
```

子组件：

```html
<template>
  <div>子组件</div>
</template>

<script setup>
  import { inject } from 'vue'

  const name = inject('name')
  console.log(name) // O.O
</script>
```

`provide/inject` 适用于祖先和后代关系的组件间的通信，祖先元素通过 `provide` 提供一个值，后代元素则通过 `inject` 获取到这个值。

如果需要设置全局变量或方法，可以将它们放在根组件的 `provide` 中，这样所有的组件都能使用它们。

如果需要变量是响应式的，使用 `ref` 或 `reactive` 包装一下。

> 推荐：[Vue 依赖注入使用 Provide 和 Inject](https://github.com/lio-zero/blog/blob/main/Vue/Vue%20%E4%BE%9D%E8%B5%96%E6%B3%A8%E5%85%A5%E4%BD%BF%E7%94%A8%20Provide%20%E5%92%8C%20Inject.md)

### mitt

Vue3 中不再有 `EventBus` 跨组件通。

因为，在 Vue3 中，Vue 不再是构造函数，而是 `Vue.createApp({})` 返回一个没有 `$on`、`$emit` 和 `$once` 方法的对象。

**怎么办？**有一个 [mitt](https://github.com/developit/mitt) 库可以帮助到我们，其原理还是 `EventBus`。

详细内容可以查阅我写的另一篇 [Vue3 使用 Event Bus](https://github.com/lio-zero/blog/blob/main/Vue/Vue3%20%E4%BD%BF%E7%94%A8%20Event%20Bus.md)。

### Vuex 和 Pinia

Vue3 的全局状态管理器有两个，第一个是 [Vuex 4](https://vuex.vuejs.org/zh/)，第二个则的 [Pinia](https://pinia.vuejs.org/)

这里不过多介绍，详细内容可以查阅官网。

> 对于 Vue 2，我写过一篇较老的文章 [Vuex](https://github.com/lio-zero/blog/blob/main/Vue/Vuex.md)，可以看看。
