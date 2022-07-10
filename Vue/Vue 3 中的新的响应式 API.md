# Vue 3 中的新的响应式 API

Vue3 使用基于 ES6 Proxy 的新的响应性系统。

> 详细内容可查阅 [Reactivity Fundamentals](https://vuejs.org/guide/essentials/reactivity-fundamentals.html) 和 [Reactivity API: Utilities](https://vuejs.org/api/reactivity-utilities.html)。

## ref()

Vue 有一个全局 `ref()` 方法，它在 JavaScript 原始类型创建一个响应式包装器。它通常只能用于基本类型：`number`、`string`、`boolean`、`bigint` 和 `symbol`。

例如，下面是如何创建一个响应式计数器对象。

```js
import { ref } from 'vue'

const count = ref(0)
console.log(count.value) // 0

++count.value

console.log(count) // 1
```

使用 Vue 的全局 [`watchEffect()`](https://vuejs.org/api/reactivity-core.html#watcheffect) 方法，您可以监视到引用的更新。

```js
import { ref, watchEffect } from 'vue'

const count = ref(0)

watchEffect(function handler() {
  console.log(count.value)
})

// 打印 1，因为 Vue 知道在计数更改时调用 handler()
++count.value
```

## reactive()

Vue3 还引入了一个 `reactive()` 方法，其行为类似于 `ref()`，但用于对象。

`reactive()` 方法的作用是：为对象的属性增加响应性。在对象上调用 `reactive()`，就会得到一个代理对象，可以使用 `watchEffect()` 监听变化。

例如，在下面的例子中，因为 `user` 是响应式的，所以 `watchEffect()` 将在每次 `user` 更改时打印出 `user.name` 的值。

```js
import { reactive, watchEffect } from 'vue'

const user = reactive({ name: 'O.O' })

watchEffect(() => {
  console.log(user.name) // 'D.O'
})

user.name = 'D.O'
```

相对于 Vue2 的 `data` 属性，`reactive()` 的最大改进是，当您添加新属性时，`reactive()` 可以监听到，而不仅仅是访问现有属性。

在下面的例子中，`watchEffect()` 足够智能，当您在 `user` 上添加一个新的属性 `age` 时，监听到行的变化：

```js
import { reactive, watchEffect } from 'vue'

const user = reactive({ name: 'O.O' })

watchEffect(() => {
  console.log(user.age) // 18
})

user.age = 18
```

`reactive()` 消除了在事件循环的同一事件上发生的变化。下面的代码将打印 61 和 62，但不会打印 59 或 60，因为这些更改同步发生在 61 之前。

```js
import { reactive, watchEffect } from 'vue'

const user = reactive({ name: 'O.O' })

watchEffect(() => {
  console.log(user.age) // 61
})

user.age = 59
user.age = 60
user.age = 61

setImmediate(() => {
  user.age = 62
}) // 62
```

## isRef()

`isRef()` 检查一个值是否是一个 ref 对象。

```js
const open = ref(true)

console.log(isRef(open)) // true
```

## unref()

如果参数是一個 `ref`，则返回内部值，否则返回参数本身。

它是 `val = isRef(val) ? val.value : val` 的语法糖函数。

```js
const open = ref(true)

console.log(unref(open)) // true
```

## toRef()

可用于为源 `reactive` 对象上的属性创建 `ref`。创建的 `ref` 与其源属性同步：改变源属性将更新 `ref`，反之亦然。

```js
const state = reactive({
  foo: 1,
  bar: 2
})

const fooRef = toRef(state, 'foo')

fooRef.value++
console.log(state.foo) // 2
console.log(unref(fooRef)) // 2
state.foo++
console.log(unref(fooRef)) // 3
```

## toRefs()

将响应式对象转换为普通对象，其中结果对象的每个属性都是指向原始对象相应属性的 `ref`。每个单独的 `ref` 都是使用 `toRef()` 创建的。

它可以在不丢失响应性的情况下对返回的对象进行解构/展开

```js
const state = reactive({
  foo: 1,
  bar: 2
})

const stateAsRefs = toRefs(state)

state.foo++
console.log(unref(stateAsRefs.foo)) // 2
stateAsRefs.foo.value++
console.log(state.foo) // 3
```

## isProxy()

`isProxy()` 检查对象是否是由 `reactive()`、`readonly()`、`shalloreactive()` 或 `shalloreadonly()` 创建的代理。

```js
const data = readonly({
  key: 123456
})

const user = reactive({
  name: 'O.O'
})

console.log(isProxy(user)) // true
console.log(isProxy(data)) // true
```

## isReactive()

检查对象是否是由 `reactive()` 或 `shallowReactive()` 创建的代理。

如果该代理是 `readonly` 创建的，但包裹了由 `reactive` 创建的另一个代理，它也会返回 `true`。

```js
const data = readonly({
  pass: 123456
})
const user = reactive({
  name: 'O.O'
})
const userCopy = readonly(user)

console.log(isReactive(user)) // true
console.log(isReactive(userCopy)) // true
```

## isReadonly()

检查对象是否是由 `readonly()` 或 `shalloreadonly()` 创建的代理。

```js
const data = readonly({
  pass: 123456
})
const user = reactive({
  name: 'O.O'
})
const userCopy = readonly(user)

console.log(isReadonly(data)) // true
console.log(isReadonly(userCopy)) // true
```

## shallowRef()

浅版本的 `ref`。如果是基本数据类型，`ref` 和 `shallowRef` 的效果一样。

```js
const state = shallowRef({ count: 1 })

state.value.count = 2
state.value = { count: 2 }
```

## triggerRef()

`triggerRef()` 配合 `shallowRef` 一起使用。

```js
const user = shallowRef({
  name: 'O.O'
})

watchEffect(() => {
  console.log(user.value.name) // O.O
})

user.value.name = 'D.O'

triggerRef(user) // D.O
```

## customRef()

创建一个自定义的 `ref`，对其依赖项跟踪和更新触发进行显式控制。

```html
<template>
  <div>name：{{ name }}</div>
  <input v-model="name" />
</template>

<script setup>
  let value = 'front-refined'

  const name = customRef((track, trigger) => {
    return {
      get() {
        // 收集依赖它的 effect
        track()
        return value
      },
      set(newValue) {
        value = newValue
        // 触发更新依赖它的所有 effect
        trigger()
      }
    }
  })
</script>
```

## shallowReactive

`reactive` 的浅版本。

```js
const state = shallowReactive({
  foo: 1,
  nested: {
    bar: 2
  }
})

state.foo++
console.log(state.foo) // 2
console.log(isReactive(state.nested)) // false

// 它是非响应性大，但值会被修改
state.nested.bar++
console.log(state.nested.bar) // 3
```

## shallowReadonly

`readonly()` 的浅版本。

```js
const state = shallowReadonly({
  foo: 1,
  nested: {
    bar: 2
  }
})

// 不会报错，只会警告
state.foo++
// 使用 shallowReadonly 代理的对象不能被修改
console.log(state.foo) // 1

// 嵌套对象的属性可以被修改，但它是非响应式的！
state.nested.bar++
console.log(state.nested.bar) // 3
```

## toRaw()

返回 Vue 创建的代理的原始对象。

```js
const open = ref(true)
const user = reactive({
  name: 'O.O'
})

console.log(toRaw(open)) // RefImpl {_shallow: false, dep: undefined, __v_isRef: true, _rawValue: true, _value: true}
console.log(toRaw(user)) // {name: 'O.O'}
```

## markRaw()

标记一个对象，使其永远不会转换为代理。返回对象本身。

```js
const foo = markRaw({})
console.log(isReactive(reactive(foo))) // false

const bar = reactive({ foo })
console.log(isReactive(bar.foo)) // false
```

## effectScope()

创建一个效果范围对象，该对象可以捕获在其中创建的响应性效果（即 `computed` 和 `watch`），以便将这些效果一起处理。

```js
const scope = effectScope()
const counter = ref(0)
scope.run(() => {
  const doubled = computed(() => counter.value * 2)

  watch(doubled, () => console.log(doubled.value))

  watchEffect(() => console.log('Count: ', doubled.value))
})

scope.stop() // 'Count: 0'
```

其他的还有：

- `getCurrentScope()` 返回当前活动的 `effect` 作用域（如果有）。
- `onScopeDispose()` 在当前活动的 `effect` 作用域上注册 `dispose` 回调。当关联的 `effect` 作用域停止时，将调用回调。

详细查阅 [Reactivity API: Advanced](https://vuejs.org/api/reactivity-advanced.html#reactivity-api-advanced)

> 关于 `watch`、`computed` 和 `watchEffect` 可以查看 [Vue Computed — 计算属性](https://github.com/lio-zero/blog/blob/master/Vue/Vue%20Computed%20%E2%80%94%20%E8%AE%A1%E7%AE%97%E5%B1%9E%E6%80%A7.md) 和 [watch 与 watchEffect 的区别](https://github.com/lio-zero/blog/blob/master/Vue/watch%20%E4%B8%8E%20watchEffect%20%E7%9A%84%E5%8C%BA%E5%88%AB.md)。
