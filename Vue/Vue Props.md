# Vue Props

Props 是任何现代 JavaScript 框架的重要组成部分。

在组件之间传递数据的能力是 Vue 项目的基本要素。在 Vue3 中，访问 `props` 的方式与以前有所不同。

## 为什么使用 Props 很重要？

[Props](https://vuejs.org/guide/components/props.html#creating-vnodes) 是可在组件上注册的自定义属性，可让我们将数据从父组件传递到其子组件之一。

由于 Props 使我们能够在组件之间共享数据，因此它使我们可以将 Vue 项目和组件组织为更多的模块化组件。

## Vue3 获取 props 的方式

在 Vue3 之前，组件的 `props` 是 `this` 对象的一部分，可以通过使用 `this.propName` 进行访问。

但是，Vue3 的一大变化是 `setup` 方法的引入。`this` 不会具有它在 Vue2 中使用过的属性。

那么我们如何不使用 `this` 来使用 Vue3 `props` 呢？

`setup` 方法有两个参数：

- `context` — 一个对象，它公开了曾经在 `this` 上的特定属性
- `props` — 包含组件的 `props` 的对象

现在，我们只需要使用 `props.propName`，不再需要 `this`。

```js
setup (props, context) {
  // context 包含了 attrs、slots 和 emit
  console.log(props.propName) // 访问组件的一个 props
}
```

## 为什么 Vue3 Props 和 Vue2 不同？

Vue3 改变访问 `props` 的方式背后的主要动机是让它在组件/方法中更清晰。有时，在查看 Vue2 代码时，`this` 所指的可能是模糊的。

Vue 团队在设计 Vue3 时的一个大目标是使其在大型项目中更具可扩展性。其中一部分是将 Options API 重新设计为 Composition API，以便更好地组织代码。

Options API 和 Composition API 对比（看颜色划分即可）：

![Options API 和 Composition API 对比](https://upload-images.jianshu.io/upload_images/18281896-f8cb2a5ff5d1e2d1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

> 关于 Composition API 的问题，可以查看 [Composition API FAQ](https://vuejs.org/guide/extras/composition-api-faq.html)。

通过去除 `this` 的引用，使用显式的 `context` 和 `props` 变量，可以提高大型 Vue 项目的可读性。

如您所知，Vue3 Props 的工作原理与 Vue2 大致相同，主要的变化是我们如何在新的 `setup` 方法中访问它们。
