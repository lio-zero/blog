# 使用导航守卫保护 Vue 路由

[Vue Router](https://router.vuejs.org/zh/) 中的守卫主要分为三类：全局守卫、路由守卫和组件守卫。

- 当触发任何导航时（即 URL 更改时），会调用全局守卫；
- 当调用关联路由时（即 URL 匹配特定路由时），会调用每个路由守卫；
- 当创建、更新或销毁路由中的组件时，会调用组件守卫。

在每个类别中，都有额外的方法，可以让您更细粒度地控制应用程序路由。下面是 Vue 路由器中每种导航保护类型中所有可用方法的快速分解。

> 以下介绍 Vue Router 4.x 提供的导航守卫。

先讲一下守卫钩子主要有 3 个参数 `to`、`from`、`next`：

- `to` — 即将进入的目标路由对象。
- `from` — 当前导航正要离开的路由。
- `next` — 控制路由的跳转。

在 Vue Router 4.x 中，`next` 不在是必须的，后面我们会讲到。

## 全局守卫

**`beforeEach`：全局前置守卫**，进入任何路由之前的操作。常用于进入页面前的权限验证，判断是否登录、页面 `title` 的修改等。

```js
router.afterEach((to, from) => {
  // ...
})
```

```js
import NProgress from 'nprogress'

router.beforeEach((to, from, next) => {
  NProgress.start()
  document.title = 'Hello World!'

  // 权限验证...
  next()
})
```

**`beforeResolve`（2.5.0+ 新增）：全局解析守卫**，确认导航之前的操作，但在所有组件守卫和异步路由组件被解析之后

```js
router.beforeResolve(async (to: any, from: any, next: any) => {
  console.log(to, from, next())
  next()
})
```

**`afterEach`：全局后置钩子**，路由解析后的操作，没有 `next` 参数，不会影响导航本身。

```js
import { isNavigationFailure, NavigationFailureType } from 'vue-router'

router.afterEach((to, from, failure) => {
  // https://next.router.vuejs.org/api/#navigationfailuretype
  if (isNavigationFailure(failure, NavigationFailureType.duplicated)) {
    console.log(failure.type)
  }

  NProgress.done()
})
```

## 路由守卫

`beforeEnter`：进入特定路由前的操作。

```js
import { createRouter, createWebHistory } from 'vue-router'

const router = createRouter({
  history:  createWebHistory(),
  routes: [
    // ...
    {
      path: '/login',
      name: 'login'
      component: Login,
      beforeEnter(to, from, next) {
        // ...
        next() // 需要调用 next，否则不会进入到该组件
      }
    }
  ]
})
```

## 组件守卫

> **注意**：Vue Router 4.x 目前还不在支持 `beforeRouteEnter` API。

![https://github.com/vuejs/router/blob/main/CHANGELOG.md](https://upload-images.jianshu.io/upload_images/18281896-6a3edf1581b94a3e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- `beforeRouteUpdate`：调用使用相同组件的新路由后的操作。常用于路由参数变化后重新请求数据。

例如：当从 `/article/1` 跳转到 `/article/2` 时，将触发 `onBeforeRouteUpdate`：

```js
import { onBeforeRouteUpdate } from 'vue-router'

onBeforeRouteUpdate(() => {
  console.log('路由更新')
})
```

- `beforeRouteLeave`：离开路由前的操作。常用于用户未保存当前页面的数据就准备跳转，离开当前路由，可以通过该钩子弹出一个提示窗口。

```js
import { onBeforeRouteLeave } from 'vue-router'

onBeforeRouteLeave(() => {
  console.log('数据未保存，确定离开？')
})
```

> **注意**：使用守卫时，有一些是需要调用 `next` 的，否则将不会进入到该页面。

## 完整的导航解析流程

以下是 Vue Router 4.x 给出了[完整的导航解析流程](https://router.vuejs.org/zh/guide/advanced/navigation-guards.html#%E5%AE%8C%E6%95%B4%E7%9A%84%E5%AF%BC%E8%88%AA%E8%A7%A3%E6%9E%90%E6%B5%81%E7%A8%8B)：

1.  导航被触发。
2.  在失活的组件里调用 `beforeRouteLeave` 守卫。
3.  调用全局的 `beforeEach` 守卫。
4.  在重用的组件里调用 `beforeRouteUpdate` 守卫(2.2+)。
5.  在路由配置里调用 `beforeEnter`。
6.  解析异步路由组件。
7.  在被激活的组件里调用 `beforeRouteEnter`。
8.  调用全局的 `beforeResolve` 守卫(2.5+)。
9.  导航被确认。
10. 调用全局的 `afterEach` 钩子。
11. 触发 DOM 更新。
12. 调用 `beforeRouteEnter` 守卫中传给 `next` 的回调函数，创建好的组件实例会作为回调函数的参数传入。

> 待完善。
