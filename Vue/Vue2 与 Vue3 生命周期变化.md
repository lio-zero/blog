# Vue2 与 Vue3 生命周期变化

## Vue2 Options API 中的生命周期钩子

以下是来自 Vue 2.x 文档中生命周期钩子的[示意图](https://cn.vuejs.org/v2/guide/instance.html#%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F%E5%9B%BE%E7%A4%BA)：

![Vue2 实例的生命周期](https://upload-images.jianshu.io/upload_images/18281896-3afae0bef1f401bf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

每个主要的 Vue 生命周期事件都分为两个钩子，分别在该事件之前和之后分别调用。

主要的四个事件，八个主要钩子如下：

- 创建 —— 在组件的创建上运行
- 挂载 —— 在挂载 DOM 时运行
- 更新 —— 修改反应性数据后运行
- 销毁 —— 在元素被销毁之前立即运行。

Vue 生命周期钩子使您有机会在 Vue 对组件执行特定操作时运行代码。Vue 为每个组件公开的钩子包括：

- `beforeCreate`
- `created`
- `beforeMount`
- `mounted`
- `beforeUpdate`
- `updated`
- `beforeDestroy`
- `destroyed`

上面的列表是有序的。因此，Vue 总是调在 `created` 之前调用 `beforeCreate`，反过来，Vue 在 `beforeMount` 之前调用 `created`。

例如：以下钩子可用于在组件完成初始渲染并创建 DOM 节点后运行代码：

```js
export default {
  mounted() {
    console.log('组件现在已挂载。')
  }
}
```

## Vue3 Composition API 中的生命周期钩子

以下是来自 Vue3 新文档中生命周期钩子的[示意图](https://vuejs.org/guide/essentials/lifecycle.html#lifecycle-diagram)：

![Vue3 实例的生命周期](https://upload-images.jianshu.io/upload_images/18281896-5ec6bff030d00cb6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在 Composition API 中，我们必须先将生命周期钩子导入我们的项目中，然后才能使用它们。这是为了使项目尽可能轻巧。

Composition API 不包括 `beforeCreate` 和 `created`（由 `setup` 方法本身代替），我们可以在 `setup` 方法中访问 9 个生命周期钩子

| `onBeforeMount`   | 在挂载开始之前调用                     |
| ----------------- | -------------------------------------- |
| `onMounted`       | 在挂载组件时调用                       |
| `onBeforeUpdate`  | 在响应性数据更改时以及重新渲染之前调用 |
| `onUpdated`       | 重新渲染后调用                         |
| `onBeforeUnmount` | 在销毁 Vue 实例之前调用                |
| `onUnmounted`     | 在实例销毁后调用                       |
| `onActivated`     | 激活 keep-alive 的组件时调用           |
| `onDeactivated`   | 停用 keep-alive 的组件时调用           |
| `onErrorCaptured` | 从子组件捕获错误时调用                 |

导入并使用它们：

```js
import { onMounted } from 'vue'

export default {
  setup() {
    onMounted(() => {
      console.log('mounted in the Composition API!')
    })
  }
}
```

## Vue2 和 Vue3 的生命周期钩子对比

| Options API     | Composition API 中钩子在 `setup` 中 |
| --------------- | ----------------------------------- |
| `beforeCreate`  | `setup()`                           |
| `created`       | `setup()`                           |
| `beforeMount`   | `onBeforeMount`                     |
| `mounted`       | `onMounted`                         |
| `beforeUpdate`  | `onBeforeUpdate`                    |
| `updated`       | `onUpdated`                         |
| `beforeDestroy` | `onBeforeUnmounted`                 |
| `destroyed`     | `onUnmounted`                       |
| `errorCaptured` | `onErrorCaptured`                   |

除了上述介绍和一些命名上的变化，具体用法差不多。Vue3 还新增了用于调试和服务端渲染场景的钩子：

- `onRenderTracked` — 调试钩子，响应式依赖被收集时调用
- `onRenderTriggered` — 调试钩子，响应式依赖被触发时调用
- `onServerPrefetch` — 组件实例在服务器上被渲染前调用

详细内容查阅新文档 [Composition API: Lifecycle Hooks](https://vuejs.org/api/composition-api-lifecycle.html)。
