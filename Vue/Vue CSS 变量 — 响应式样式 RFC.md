# Vue CSS 变量 — 响应式样式 RFC

[单文件组件样式 RFC](https://github.com/vuejs/rfcs/pull/231) 为 Vue 开发人员提供了一种将组件的响应数据用作 CSS 变量的方法。

## 使用 SFC 样式

- 声明响应式数据
- 使用 `v-bind` 在我们的 CSS 中访问它们

```html
<template>
  <div>
    <div class="text">hello</div>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        color: 'red'
      }
    }
  }
</script>

<style>
  .text {
    color: v-bind(color);
  }
</style>
```

更复杂的数据结构，假设我们有一个名为 `font` 的对象，并且其中有一个名为 `weight` 的属性。

```html
<script>
  export default {
    data() {
      return {
        color: 'red',
        font: {
          weight: 700
        }
      }
    }
  }
</script>

<style>
  .text {
    color: v-bind(color);
    /* 用引号括起来 */
    font-weight: v-bind('font.weight');
  }
</style>
```

> **注意**：我们必须用引号将我们的表达式括起来。

Vue 提供了内置的响应式系统，我们只需要修改响应式中的数据，便可以动态的修改页面中的外观。

```html
<div>
  <div class="text">hello</div>
  <button @click="color = 'plum'">Make Plum</button>
</div>
```

## Vue SFC 样式变量如何工作？

**使用到了 CSS 变量**，我们样式中的 `v-bind` 最终将被编译为使用 CSS `var` 语法和我们的新 CSS 变量。

> **注意**：为避免继承问题，在父组件定义的 CSS 变量对其任何子组件均不可用。

## 使用前检查浏览器支持

如果您希望在生产环境中实现这一点，请确保检查浏览器对 CSS 变量的支持。

如果没有 CSS 变量，SFC 样式变量将无法工作，因此如果您的应用程序需要支持某些较旧的浏览器，这对您来说可能不是一个可行的选择。

以下是 [CSS 变量](https://caniuse.com/css-variables)支持情况：

![css variables](https://upload-images.jianshu.io/upload_images/18281896-c37c14cc9a39ee78.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
