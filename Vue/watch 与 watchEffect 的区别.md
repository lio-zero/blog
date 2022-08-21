# watch 与 watchEffect 的区别

在开发 Vue 应用程序的整个过程中，您将拥有大量的响应性数据属性。您的应用程序将跟踪 `input` 字段、`data` 计算和一系列其他属性，并且可能需要在值更新时执行操作。

Vue 中的 watcher 可以观察响应性属性，并可以检测到属性何时发生变化。它本质上充当组件响应式数据的事件监听器。

特别是当与异步 API 调用配合使用时，例如：

- ID 更改时从数据库获取对象
- `prop` 更改时重新运行动画
- 监听路由变化
- etc

在 Vue3 中，除了 [`watch`](https://vuejs.org/api/reactivity-core.html#watch) 方法之外，还有一个新的 [`watchEffect`](https://vuejs.org/api/reactivity-core.html#watcheffect) 方法，可以在 Composition API 中使用。下面我们来简单介绍一下这两个方法。

## 如何使用 Vue watch？

[Vue Options API](https://vuejs.org/api/) 提供了一个 `watch` 选项，您可以在其中定义监视对象。要使用它，首先必须在 `data` 对象中具有要跟踪的属性。

```js
export default {
  data() {
    return {
      title: 'Vue2'
    }
  },
  watch: {
    // Watcher
  }
}
```

然后，我们必须查看 watcher 方法的结构。您所要做的就是声明一个与要观察的属性同名的函数。

它应该带有两个参数：

- 被监听属性的新 `value`
- 被监听属性的旧 `value`

例如，这是 `title` 属性的 watcher。

```js
watch: {
  title(newTitle, oldTitle) {
    console.log(`标题从 ${oldTitle} 更改为 ${newTitle})
  }
}
```

每当 `title` 值发生变化时，我们都可以在控制台中看到以下内容。

![watch title](https://upload-images.jianshu.io/upload_images/18281896-15c48e35d55ec856.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

Vue watcher 只跟踪一个依赖项，**但如果我们需要跟踪两个依赖项呢**？

在 Vue2 中，我们可以为每个方法创建两个相同的 watcher，但 Vue3 Composition API 为该用例提供了更好的解决方案，它就是 `watchEffect`。

## Vue `watchEffect` 是如何工作的？

`watchEffect` 是 Vue3 中跟踪响应依赖关系的方法之一。本质上，我们可以使用响应性属性编写一个方法，**每当它们的任何值更新时**，我们的方法就会运行。

需要注意的一点是，除了在依赖项更改时运行外，**每当组件初始化时，`watchEffect` 也会立即运行**，因此在装载 DOM 之前尝试访问 DOM 时要小心。

### 简单示例

让我们看一个非常简单的示例。

假设我们有某种响应性的 `userID` 属性，并且只想在它发生变化时打印出来。

```html
<script setup>
  import { ref, watchEffect } from 'vue'

  const userID = ref(0)
  watchEffect(() => console.log('Value: ' + userID.value))
  setTimeout(() => {
    userID.value = 1
  }, 1000)
</script>
```

当我们的组件初始化时，`watchEffect` 将运行并记录起始值。然后，每当 `userID` 的值改变时，就会触发 `watchEffect`。

![watchEffect](https://upload-images.jianshu.io/upload_images/18281896-80e8a547e8a4536b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 为什么与 watch 不同？

那么，既然已经有了 `watch` 方法，为什么这个新的 `watchEffect` 方法还会存在呢?

有一些关键的区别：

- `watchEffect` 将在其**任何**依赖项更改时运行方法，`watch` 跟踪一个或多个**特定**的响应性属性，并且仅在该属性发生更改时运行。
- 默认情况下，`watch` 是惰性的，因此仅当依赖项更改时才会触发。`watchEffect` 在创建组件后立即运行，然后跟踪依赖关系。

但是，我们也可以为 `watch` 传递一个 `immediate` 属性，使其在初始化时运行。

```html
<script setup>
  import { ref, watch } from 'vue'

  watch(
    title,
    (newTitle, oldTitle) => {
      console.log(`标题从 ${oldTitle} 更改为 ${newTitle}`)
    },
    {
      immediate: true
    }
  )
</script>
```

`watch` 在以下情况下很有用：

- 您需要控制哪些依赖项会触发该方法
- 您需要访问之前的值

这需要看您的项目适合哪一种。

## 对于 `watcheffect` 需要注意的事项

以下整理 [Vue 文档对于 WatchEffect](https://v3.cn.vuejs.org/guide/reactivity-computed-watchers.html#watcheffect) 需要注意的事项。

### props 是响应的

这是一个方便的小知识。一个很好的用例是根据 `prop` 属性更改页面上的其他值。

例如，假设我们有一个 `SongPlayer` 组件，该组件接受一个 `songID` 值作为 `prop`。如果我们更改了歌曲，我们将希望加载有关它的所有内容。我们可以用 `prop` 来做到这一点。

```html
<script setup>
  import { ref, watchEffect } from 'vue'

  const props = defineProps({
    songID: String
  })

  watchEffect(() => console.log(props.songID))
</script>
```

### 使用 `onInvalidate()` 清除副作用

使用 Vue `watchEffect` 的一个常见用例是，每当响应性依赖项发生变化时，就进行某种异步 API 调用。但是，如果依赖关系在第一个 API 调用完成之前再次发生变化，会发生什么呢？

**这就是无效副作用出现的原因。**

我们的 `watchEffect` 方法还有一个 `onInvalidate` 方法，每当该方法要再次运行或监视程序停止时，该方法就会运行。

我们可以像这样停止异步 API 调用。

```html
<script setup>
  import { watchEffect } from 'vue'

  const props = defineProps({
    songID: String
  })

  watchEffect((onInvalidate) => {
    // 异步 API 调用
    const apiCall = someAsyncMethod(props.songID)

    onInvalidate(() => {
      // 取消 API 调用
      apiCall.cancel()
    })
  })
</script>
```

### 停止观察者

我们还可以手动阻止一个观察者。如果我们想观察一个依赖项，直到它达到某个值，然后停止它，这可能很有用。如果我们在它达到目标值后继续观察，我们只是在**浪费资源**。

我们的 Vue `watchEffect` 方法返回一个 `stop` 方法，该方法将停止我们的观察程序。

因此，我们可以简单地将其存储为变量，并在稍后调用它。

```html
<script setup>
  import { watchEffect } from 'vue'

  const props = defineProps({
    songID: String
  })

  let stopWatcher = watchEffect(() => console.log(props.songID))
  stopWatcher()
</script>
```
