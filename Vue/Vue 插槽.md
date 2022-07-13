# Vue 插槽

本文参考官网的[插槽](https://vuejs.org/v2/guide/components-slots.html)，你可以在 [CodeSandbox](https://codesandbox.io/s/competent-black-vdp1l?file=/src/components/Home.vue) 测试效果，基本模板已经给出。

> 在 2.6.0 中，具名插槽和作用域插槽引入了一个新的统一的语法 (即 `v-slot` 指令)。它取代了 `slot` 和 `slot-scope` 这两个目前已被废弃但未被移除且仍在[文档中](https://cn.vuejs.org/v2/guide/components-slots.html#%E5%BA%9F%E5%BC%83%E4%BA%86%E7%9A%84%E8%AF%AD%E6%B3%95)的 attribute。新语法的由来可查阅这份 [RFC](https://github.com/vuejs/rfcs/blob/master/active-rfcs/0001-new-slot-syntax.md)。

## 默认插槽（单个插槽）

使用 `<slot>` 标签来提供一个占位符的作用，父组件可以在使用该组件时，传递一些内容。

```html
<!-- nav.vue -->
<div>
  <h2>我是一个标题</h2>
  <slot> 仅在没有插槽内容时显示 </slot>
</div>
```

你可以在 `<slot></slot>` 添加默认内容，它只会在没有提供内容的时候被渲染。

将组件与数据用于插槽：

```html
<nav>
  <p>这个会放在 slot 里</p>
</nav>
```

当组件渲染的时候，`<slot></slot>` 将会被替换为你指定的内容。

> **注意**：父级模板里的所有内容都是在父级作用域中编译的；子模板里的所有内容都是在子作用域中编译的。

## 具名插槽（多个插槽）

> 自 2.6.0 起有所更新。已废弃的使用 `slot` 属性的语法在[这里](https://cn.vuejs.org/v2/guide/components-slots.html#%E5%BA%9F%E5%BC%83%E4%BA%86%E7%9A%84%E8%AF%AD%E6%B3%95)。

对于这样的情况，`<slot>` 元素有一个特殊的属性：`name`。这个属性可以用来定义额外的插槽：

```html
<div class="container">
  <header>
    <slot name="header"></slot>
  </header>
  <main>
    <slot>Default content</slot>
  </main>
  <footer>
    <slot name="footer"></slot>
  </footer>
</div>
```

一个不带 `name` 的 `<slot>` 出口会带有隐含的名字 `default`。

在向具名插槽提供内容的时候，我们可以在一个 `<template>` 元素上使用 `v-slot` 指令，并以 `v-slot` 的参数的形式提供其名称：

```html
<Layout>
  <template v-slot:header>
    <h1>Here might be a page title</h1>
  </template>

  <!-- 默认插槽 -->
  <p>A paragraph for the main content.</p>
  <p>And another one.</p>
  <!-- 或
    <template v-slot:default>
      <p>A paragraph for the main content.</p>
      <p>And another one.</p>
    </template>
  -->

  <template v-slot:footer>
    <p>Here's some contact info</p>
  </template>
</Layout>
```

现在 `<template>` 元素中的所有内容都将会被传入相应的插槽。任何没有被包裹在带有 `v-slot` 的 `<template>` 中的内容都会被视为默认插槽的内容。

它将替换为以下内容：

```html
<div class="container">
  <header>
    <h1>Here might be a page title</h1>
  </header>
  <main>
    <p>A paragraph for the main content.</p>
    <p>And another one.</p>
  </main>
  <footer>
    <p>Here's some contact info</p>
  </footer>
</div>
```

注意 **`v-slot` 只能添加在 `<template>` 上**（只有[一种例外情况](https://cn.vuejs.org/v2/guide/components-slots.html#%E7%8B%AC%E5%8D%A0%E9%BB%98%E8%AE%A4%E6%8F%92%E6%A7%BD%E7%9A%84%E7%BC%A9%E5%86%99%E8%AF%AD%E6%B3%95)），这一点和已经废弃的 [`slot` 属性](https://cn.vuejs.org/v2/guide/components-slots.html#%E5%BA%9F%E5%BC%83%E4%BA%86%E7%9A%84%E8%AF%AD%E6%B3%95) 不同。

## 作用域插槽

> 自 2.6.0 起有所更新。已废弃的使用 `slot-scope` 属性的语法在[这里](https://cn.vuejs.org/v2/guide/components-slots.html#%E5%BA%9F%E5%BC%83%E4%BA%86%E7%9A%84%E8%AF%AD%E6%B3%95)。

有时让插槽内容能够访问子组件中才有的数据是很有用的。

下面例子中，为了让 `user` 在父级的插槽内容中可用，我们可以将 `user` 作为 `<slot>` 元素的一个属性绑定上去：

```html
<header>
  <slot name="header" :user="user"></slot>
</header>
```

绑定在 `<slot>` 元素上的属性被称为**插槽 prop**。现在在父级作用域中，我们可以使用带值的 `v-slot` 来定义我们提供的插槽 prop 的名字：

```html
<Layout>
  <template v-slot:header="{ user }">
    {{ user }}
    <h1>我在父组件头部传递内容给子组件的插槽</h1>
  </template>
</Layout>
```

在这个例子中，我们选择将包含所有插槽 prop 的对象命名为 `slotProps`，你可以随意命名，也可以解构它：

```html
<Layout>
  <template v-slot:header="{ user }">
    {{ user }}
    <h1>我在父组件头部传递内容给子组件的插槽</h1>
  </template>
</Layout>
```

当只有默认插槽时，你可以这么写：

```html
<Layout v-slot="slotProps">
  {{ slotProps.user }}
  <h1>我在父组件头部传递内容给子组件的插槽</h1>
</Layout>
```

我们可以直接将 `v-slot` 用在组件上，但注意默认插槽的缩写语法**不能**和具名插槽混用，因为它会导致作用域不明确：

```html
<!-- 无效，会导致警告 -->
<Layout v-slot="{ user }">
  {{ user }}
  <template v-slot:header="header"> header 在这里无法使用 </template>
</Layout>
```

只要出现多个插槽，请始终为所有的插槽使用完整的基于 `<template>` 的语法：

```html
<Layout>
  <template v-slot:header="{ user }"> {{ user }} </template>

  <template v-slot:footer="{ footer }"> {{ footer }} </template>
</Layout>
```

## 动态插槽名（2.6.0 新增）

[动态指令参数](https://cn.vuejs.org/v2/guide/syntax.html#%E5%8A%A8%E6%80%81%E5%8F%82%E6%95%B0)也可以用在 `v-slot` 上，来定义动态的插槽名：

```html
<Layout>
  <template v-slot:[dynamicSlotName]> ... </template>
</Layout>
```

## 具名插槽的缩写（2.6.0 新增）

`v-slot` 的缩写为 `#`，例如 `v-slot:header` 可以的简写为 `#header`：

```html
<Layout>
  <template #header>
    <h1>Here might be a page title</h1>
  </template>
</Layout>
```

该简写只在其有参数的时候才可用。以下语法是无效的：

```html
<!-- 这样会触发一个警告 -->
<current-user #="{ user }"> {{ user }} </current-user>
```
