# Vue 错误处理 — onErrorCaptured 钩子

Vue 实例有一个 [`onErrorCaptured`](https://vuejs.org/api/composition-api-lifecycle.html#onerrorcaptured) 钩子，每当事件处理程序或生命周期钩子抛出错误时，Vue 会调用该钩子。

例如，下面的代码将增加一个计数器，每次单击按钮时，组件 `test` 都会抛出一个错误。

```html
<template>
  <span id="count">{{ count }}</span>
  <test></test>
</template>

<script setup>
  import { onErrorCaptured, ref } from 'vue'
  import test from './test.vue'

  const count = ref(0)

  onErrorCaptured((err) => {
    console.log('Caught error', err.message)
    ++count.value
    return false
  })
</script>
```

`test.vue` 组件内容如下：

```html
<!-- 这里我们没有提供 notAMethod 方法供按钮调用 -->
<template>
  <button @click="notAMethod()">Throw</button>
</template>
```

按照上面的步骤走，您将在控制台看到 `onErrorCaptured` 捕获的错误：

```txt
Caught error _ctx.notAMethod is not a function
```

[示例地址](https://sfc.vuejs.org/#eNp9Ur1u1EAQfpVhm/Mp/qE++SyiEyUd5TaOPb5zsHdXs+ujsFxRQKRI/AiQEDR0FAghqBASvExkkrdgd32B5CKlmr9vv29mdnp2qFS87ZAtWGqwVU1uMOMCINUqF1CXS84K2QnDWdb34F0YhjRx5QloUJssTbzhwjn/aGyoC6qVAY2mUx5ft0qSgR5KrGqBK2ljgcKEIMV9IkmrXJmOsAyBsIIBKpItzGyPsyvPndquEntpN4QFOEghhS1OrS4dSXB37vJ7gkHvkuCpnDdYkLN7bQQBEs1hmcEO7+hlg3Ej18FslXfrjQELkTQLnY1b1Dpfo5cEODjwjcTbvOlwSpHdBQmo8kb7jBO2C/WLyljIphGjNlfxsZbCfo1X5ruC5mxx2QtndmwXc7YxRulFkuiqcLs41rGkdWK9mKx+3WKMuo2OSD7WSJaYs/AKR2KTW6SIUJRISLdx7kFv8PqZuBjsKJc/487rThTB+e93F09Px2cvz35+Hr99HD+cjM9fnP16D0KawwdoNrKE8e2P8fsbmxxPTy5efTn/+uTP608QRe64rp/oUWeMFHCvaOrikT3U/yTB3N7rw41tKk0m1PXTZMNf5AMWgQ==)

## `onErrorCaptured` 只捕获嵌套组件中的错误

一个常见的问题是，当错误发生在注册 `onErrorCaptured` 钩子的同一组件中时，Vue 不会调用 `onErrorCaptured`。

例如，如果从上述示例中删除 `test` 组件，并将按钮内联到顶级 Vue 实例中，Vue 将不会调用 `onErrorCaptured`。

```html
<template>
  <span id="count">{{ count }}</span>
  <button @click="notAMethod">Throw</button>
</template>

<script setup>
  import { onErrorCaptured, ref } from 'vue'
  import test from './test.vue'

  const count = ref(0)

  // Vue 不会调这个钩子，因为错误发生在这个 Vue 实例中，而不是子组件。
  onErrorCaptured((err) => {
    console.log('Caught error', err.message)
    ++count.value
    return false
  })
</script>
```

## 异步错误

好的一面是，当异步函数抛出错误时，Vue 会调用 `onErrorCapture`。

例如，如果子组件异步抛出错误，Vue 仍然会将错误冒泡给父组件。

```html
<template>
  <span id="count">{{ count }}</span>
  <test />
</template>

<script setup>
  import { onErrorCaptured, ref } from 'vue'
  import test from './test.vue'

  const count = ref(0)

  onErrorCaptured((err) => {
    console.log('Caught error', err.message)
    ++count.value
    return false
  })
</script>
```

`test.vue`：

```html
<template>
  <button @click="test">Throw</button>
</template>

<script setup>
  // Vue 会将异步错误冒泡到父级的 onErrorCaptured()，因此每次单击该按钮时，
  // Vue 都会调用带有 err 的 onErrorCaptured() 钩子。
  // err.message = 'Oops'
  const test = async () => {
    await new Promise((resolve) => setTimeout(resolve, 50))
    throw new Error('Oops!')
  }
</script>
```

控制台将打印：

```txt
Caught error Oops!
```

## 错误传播

在前面的示例中，您可能已经注意到 `return false`。如果 `onErrorCaptured()` 函数没有 `return false`，则 Vue 会将错误冒泡到父组件的 `onErrorCaptured()`：

```html
<template>
  <span id="count">{{ count }}</span>
  <test1 />
</template>

<script setup>
  import { onErrorCaptured, ref } from 'vue'
  import test1 from './test1.vue'

  // 由于 test1 组件的 onErrorCaptured() 没有 return false，Vue 将冒泡显示错误。
  const count = ref(0)

  onErrorCaptured((err) => {
    console.log('Caught top-test error', err.message)
    ++count.value
    return false
  })
</script>
```

`test1.vue`：

```html
<template>
  <test2 />
</template>

<script setup>
  import { onErrorCaptured, ref } from 'vue'
  import test2 from './test2.vue'

  onErrorCaptured((err) => {
    console.log('test 1 error', err.message)
  })
</script>
```

`test2.vue`：

```html
<template>
  <button @click="notAMethod()">Throw</button>
</template>
```

![错误传播](https://upload-images.jianshu.io/upload_images/18281896-6b893d29b97b1a74.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

另一方面，如果 `onErrorCaptured()` 方法使用 `return false`，Vue 将停止该错误的传播：

```js
// test2.vue
onErrorCaptured((err) => {
  console.log('test 1 error', err.message)
  return false
})
```

## 最后

您可以继续阅读 [Handling Errors in Vue with Error Boundaries](https://medium.com/@dillonchanis/handling-errors-in-vue-with-error-boundaries-91f6ead0093b) 这篇文章，它利用 `onErrorCaptured` 钩子封装了一个类似 React [`ErrorBoundary`](https://reactjs.org/blog/2017/07/26/error-handling-in-react-16.html) 的组件。[组件地址](https://github.com/dillonchanis/vue-error-boundary)。
