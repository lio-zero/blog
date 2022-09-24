# Vuex

[Vuex](https://vuex.vuejs.org/zh/) 是一个专为 Vue.js 应用开发的** + 库**。它采用集中式存储管理应用的所有组件的状态，并以相应的规则保证状态以一种可预测的方式发生变化。

简单的说，它是实现组件全局状态（数据）管理的一种机制，可以方便的实现组件之间的数据共享。

![vuex](https://upload-images.jianshu.io/upload_images/18281896-8a572061902274a7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

> **注意**：本文为 Vue 2.x 写法，详细的语法变更请查阅 [Vuex 4.x 文档](https://vuex.vuejs.org/zh/)。此外，Vue 3.x 的下一代 Vuex 开发重心已转移到 [pinia](https://github.com/vuejs/pinia)，它支持 Vue 2.x 和 Vue 3.x 版本。

## 安装

```bash
$ npm i vuex
# or
$ yarn add vuex
```

在 vue 实例中注册 `store`：

```js
import store from './store'

const app = createApp(App)

// ...
app.use(store)
```

通过在根实例中注册 `store` 选项，该 store 实例会注入到根组件下的所有子组件中，且子组件能通过 `this.$store` 访问到。

你可以在 👉 [Store](https://codesandbox.io/s/sb7vb) 测试效果。

## State

State 提供唯一的公共数据源，所有共享的数据都要统一放到 Store 的 state 中进行储存

```js
state: {
  counter: 1,
  msg: 'hello world!'
}
```

**获取方式二**：

使用 `mapState` 函数，将全局数据映射为当前组件的 `computed` 计算属性使用

```js
import { mapState } from 'vuex'

...mapState(['counter', 'msg'])
```

## Mutations

Mutation 用于变更 Store 中的状态，但它总是同步的。

- Mutation 是改变存储中状态数据的唯一方法。
- 通过这种方式虽然操作起来稍微繁琐一些，但是可以集中监控所有数据的变化

```js
mutations: {
    // 定义事件处理函数
  increment(state) {
    // 变更状态
    state.counter++
  },
  // 可以在触发 mutations 时传递参数：
  incrementN(state, step) {
    state.counter += step
  },
  decrementN(state, step) {
    state.counter += step
  }
}
```

**调用方式一**：

```js
// 调用 commit 函数
this.$store.commit('increment')
// 触发 mutations 时携带参数
this.$store.commit('incrementN', 3)
```

**调用方式二**：

使用 `mapMutations` 函数，映射为当前组件的 `methods` 函数

```js
import { mapMutations } from 'vuex'

...mapMutations(['increment', 'incrementN'])
```

## Actions

Action 将允许我们异步更新状态，但将使用现有的 mutation。如果你需要按照特定的顺序同时执行几个不同的 mutation，这会非常有帮助。

```js
actions: {
  asyncDecrementN({ commit }, asyncNum) {
    setTimeout(() => {
      commit("decrementN", asyncNum.by)
    }, asyncNum.duration)
  }
}
```

**获取异步方法**：

```js
// 触发 actions 异步任务时携带参数
asyncDecrementN() {
  this.$store.dispatch("asyncDecrementN", {
    by: -3,
    duration: 1000,
  })
},
```

## Getters

Getter 可以对 Store 中已有的数据加工处理之后形成新的数据，类似 Vue 的计算属性

- Getter 将使值能够在模板中静态显示。换句话说，getter 可以读取值，但不能改变状态。
- Store 中数据发生变化，Getter 的数据也会跟着变化

```js
// 展示数据，而不是更改状态
getters: {
  tripleCounter: (state) => state.counter * 3
}
```

**获取方式一**：

```js
$store.getters.tripleCounter
```

**获取方式二**：

使用 `mapGetters` 函数，将全局数据映射为当前组件的计算属性使用

```js
import { mapGetters } from 'vuex'

computed: {
  ...mapGetters(['tripleCounter'])
}
```

## Modules

> 由于使用单一状态树，应用的所有状态会集中到一个比较大的对象。当应用变得非常复杂时，store 对象就有可能变得相当臃肿。

为了解决以上问题，Vuex 允许我们将 store 分割成**模块（module）**。每个模块拥有自己的 `state`、`mutation`、`action` 和 `getter`。

```js
// module/moduleA.js
const moduleA = {
 state: () => ({
 name: "李四"
 }),
 mutations: {},
 actions: {},
 getters: {}
};

export default moduleA;

// module/moduleB.js
const moduleB = {
 state: () => ({
 name: "老六"
 }),
 mutations: {},
 actions: {}
};

export default moduleB;

// store/index.js
import moduleA from "./module/moduleA"
import moduleB from "./module/moduleB"
const store = new Vuex.Store({
 modules: {
 moduleA,
 moduleB
 }
})

store.state.moduleA // -> moduleA 的状态
store.state.moduleB // -> moduleB 的状态
```

详细信息请看 [modules](https://vuex.vuejs.org/zh/guide/modules.html)。

## 其他

### 热重载

Vuex 支持在开发过程中[热重载](https://vuex.vuejs.org/zh/guide/hot-reload.html#%E5%8A%A8%E6%80%81%E6%A8%A1%E5%9D%97%E7%83%AD%E9%87%8D%E8%BD%BD) mutation、module、action 和 getter。

### 项目结构

Vuex 并不限制你的代码结构。但是，它规定了一些需要遵守的规则：

- 应用层级的状态应该集中到单个 store 对象中。
- 提交 **mutation** 是更改状态的唯一方法，并且这个过程是同步的。
- 异步逻辑都应该封装到 **action** 里面。

如果你的 store 文件太大，只需将 action、mutation 和 getter 分割到单独的文件。对于大型应用，我们会希望把 Vuex 相关代码分割到模块中。下面是项目结构示例：

```bash
└── store
    ├── index.js          # 我们组装模块并导出 store 的地方
    ├── actions.js        # 根级别的 action
    ├── mutations.js      # 根级别的 mutation
    └── modules
        ├── cart.js       # 购物车模块
        └── products.js   # 产品模块
```

### 插件

Vuex 的 store 接受 `plugins` 选项，这个选项暴露出每次 mutation 的钩子。Vuex 插件就是一个函数，它接收 store 作为唯一参数：

```js
const myPlugin = (store) => {
  // 当 store 初始化后调用
  store.subscribe((mutation, state) => {
    // 每次 mutation 之后调用
    // mutation 的格式为 { type, payload }
  })
}
```

然后像这样使用：

```js
const store = new Vuex.Store({
  // ...
  plugins: [myPlugin]
})
```

### 表单处理

在 Vuex 严格模式下，状态变更需要在 mutation 函数下执行。在你不了解这个的情况下，使用 `v-model` 时，可能会发生一些错误。

```html
<input v-model="obj.message" />
```

假设这里的 `obj` 是在计算属性中返回的一个属于 Vuex store 的对象，在用户输入时，`v-model` 会试图直接修改 `obj.message`。在严格模式中，由于这个修改不是在 mutation 函数中执行的, 这里会抛出一个错误。

**方式一**：给 `<input>` 中绑定 value，然后侦听 `input` 或者 `change` 事件，在事件回调中调用一个方法

```html
<input :value="message" @input="updateMessage" />

<script>
  export default {
    computed: {
      ...mapState({
        message: (state) => state.obj.message
      })
    },
    methods: {
      updateMessage(e) {
        this.$store.commit('updateMessage', e.target.value)
      }
    }
  }
</script>
```

**mutation 函数**：

```js
mutations: {
  updateMessage (state, message) {
    state.obj.message = message
  }
}
```

**方式二**：使用 setter 的双向绑定计算属性

```js
computed: {
  message: {
    get () {
      return this.$store.state.obj.message
    },
    set (value) {
      this.$store.commit('updateMessage', value)
    }
  }
}
```

### 严格模式

开启严格模式，仅需在创建 store 的时候传入 `strict: true`：

```js
const store = new Vuex.Store({
  // ...
  strict: true
})
```

在严格模式下，无论何时发生了状态变更且不是由 `mutation` 函数引起的，将会抛出错误。这能保证所有的状态变更都能被调试工具跟踪到。

**不要在生成环境下启用严格模式**！严格模式会深度监测状态树来检测不合规的状态变更，请确保在生产环境下关闭严格模式，以避免性能损失。

类似于插件，我们可以让构建工具来处理这种情况：

```js
const store = new Vuex.Store({
  // ...
  strict: process.env.NODE_ENV !== 'production'
})
```

## 常见的问题

### 谈谈你对 Vuex 的理解？

理解如下两张图，结合官网对 Vuex 的解释（[Vuex 是什么？](https://vuex.vuejs.org/zh/)）：

![flow](https://vuex.vuejs.org/flow.png)

![vuex](https://vuex.vuejs.org/vuex.png)

### 什么时候使用 Vuex？

Vuex 通过全局注入 store 对象，来实现组件间的状态共享。

开发大型单页应用时（多级组件嵌套），需要实现一个组件更改某个数据，多个组件自动获取更改后的数据进行业务逻辑处理，这时候使用 vuex 比较合适。

如果只是简单的多个组件间传递数据，可以使用组件间常用的通信方法。

### Vuex 的设计思想

Vuex 借鉴了 [Flux](https://facebook.github.io/flux/docs/overview)、[Redux](http://redux.js.org/) 和 [The Elm Architecture](https://guide.elm-lang.org/architecture/)，将数据存放到全局的 store，再将 store 挂载到每个 vue 实例组件中，利用 Vue.js 的细粒度数据响应机制来进行高效的状态更新。

### Vuex 统一管理状态的好处

- 能够在 Vuex 中集中管理共享的数据，易于开发和后期维护
- 能够高效地实现组件之间的数据共享，提高开发效率
- 存储在 Vuex 中的数据都是响应式的，能够实时保存数据与页面的同步

### Mutation 和 Action 有什么区别？

- mutation 是 Vuex 用于修改 store 中状态的唯一方法，而 action 只能通过 mutation 修改状态。
- action 可以包含任意异步操作，而 mutation 只能是同步操作
- 它们提交的方式不同，mutation 使用 `commit` 方法，而 action 使用 `dispatch` 方法。
- 这些你都可以在官网找到 — [Mutation](https://vuex.vuejs.org/zh/guide/mutations.html) 和 [Action](https://vuex.vuejs.org/zh/guide/actions.html)
