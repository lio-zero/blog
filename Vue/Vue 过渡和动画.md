# Vue 过渡和动画

**Vue 过渡和动画是使您的网站**更具现代感并为网站访问者提供更好的用户体验的好方法。

为了适应广泛的开发人员，Vue 提供了几种方法来实现过渡：

- CSS 过渡/动画样式
- JavaScript 钩子可以对 DOM 进行编辑
- 使用第三方 CSS/JS 库

本文将介绍如何在 Vue 中使用它们。

## 什么是 `<transition>` 元素？

`transition` 元素是一个内置组件包装器，可以帮助您向元素添加过渡功能。本质上，它设置了不同的钩子，并向不断变化的元素添加类，以便我们可以在过渡的不同阶段对它们进行样式设置。

有 6 个不同的过渡类（3 个用于进入，3 个用于离开）。

- `v-enter-from` / `v-leave-from`：过渡的开始状态；过渡开始后将其删除（在 Vue2 中，它们是 `v-enter`和`v-leave`）
- `v-enter-active` / `v-leave-active`：过渡的活动状态
- `v-enter-to` / `v-leave-to`：过渡的结束状态

> **注意**：给 `transition` 添加 `name` 属性时，类的格式是`{name}-enter-from`，`{name}-enter-active`，依此类推。

我们来看一个简单的示例。

## Vue 过渡示例

使用 `<transition>` 内置组件包裹需要过渡的元素，并将 `name` 属性值设置为已定义好的类 ：

```html
<template>
  <h1>Vue 过渡动画</h1>
  <button @click="open = !open">切换动画</button>
  <transition name="fade">
    <p v-if="open" class="example-div">Hello Vue</p>
  </transition>
</template>

<script setup>
  import { ref } from 'vue'
  const open = ref(true)
</script>
```

添加过渡样式：

```css
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
```

注意这些类前面的 `fade` 前缀，它是我们在 `transition` 标签上使用 `name` 属性的值。

以上示例中，当过渡处于活动状态时，向 `opacity` 属性添加过渡，使其平滑移动。

## 自定义类名和 JS 钩子

我们还可以通过将以下 6 个属性中的任何一个添加到我们的 `<transition>` 元素中来覆盖这些默认类的任何一个：

- `enter-from-class`
- `enter-active-class`
- `enter-to-class`
- `leave-from-class`
- `leave-active-class`
- `leave-to-class`

这在将自定义库添加到代码中时，这特别有用，例如 Animate.css，后面我们会介绍。

```html
<transition
  enter-active-class="animate__animated animate__fadeIn animate__zoomIn"
  leave-active-class="animate__animated animate__fadeOut animate__zoomOut"
>
  ...
</transition>
```

另外， `transition` 元素还会触发 JS 钩子，因此我们可以捕获它们，并使用 JavaScript（而不是 CSS）执行动画。可用的钩子是：

- `before-enter / before-leave`
- `enter / leave`
- `after-enter / after-leave`
- `enter-cancelled / leave-cancelled`

```html
<transition @before-enter="beforeEnter">...</transition>
```

然后，我们可以在 JavaScript 中处理它们。

```js
// done 是一个可选的回调方法
beforeEnter(el, done) {
  done()
}
```

## 使用 Vue 过渡的高级用法

### 让您的组件在加载时进行过渡

我们只需将 `appear` 属性添加到您的过渡元素中。

```html
<transition name="fade" appear>...</transition>
```

### 在多个元素之间过渡

假设您有两个像这样彼此交替的 `div`。

```html
<transition name="fade" appear>
  <div v-if="visible">Option A</div>
  <div v-else>Option B</div>
</transition>
```

你所要做的就是把它们包装在一个过渡元素中。要使代码按您想要的方式运行，需要注意以下几点：

- **绝对定位元素** — 当 Vue 在这两个元素之间转换时，有时两个元素都是可见的，并转换为 `in/out`。如果你想要一个平滑的效果，您可能希望将它们绝对放置在彼此之上。否则，当元素从 DOM 中添加/删除时，它们可能只是到处跳跃。
- **如果元素具有相同的标签，则必须向组件添加 `key` 属性** 如果您的元素具有相同的标签，Vue 将尝试优化内容，只替换元素的内容。根据文档，如果在多个元素之间转换，最好添加一个键。

## 自定义过渡时间

Vue 通常可以检测到您的过渡/动画结束的时间，但是如果您想设置确切的持续时间，Vue 过渡有一个 `duration` 属性。

您既可以为 `enter` 和 `leave` 过渡都传递一个值，也可以传递具有两个值的对象。

```html
<transition :duration="500">...</transition>

<transition :duration="{ enter: 1000, leave: 200 }">...</transition>
```

## 动态组件之间的过渡

您只需要将 Vue 动态组件包装在 `transition` 元素中。

```html
<transition name="fade" appear>
  <component :is="componentType" />
</transition>
```

### 创建可重用的 Vue 过渡组件

在 Vue 中工作时要养成的一个好习惯是尝试设计可重用的组件。

我们只需要将 `transition` 元素放在根中并插入组件插槽，以便我们可以添加更多内容。

```html
<template>
  <transition name="fade" appear>
    <slot></slot>
  </transition>
</template>
```

现在，您不必担心将过渡样式、名称和所有内容添加到每个组件中，而只需使用此组件即可。

## 第三方库

为了方便使用 Vue 过渡效果，我们可以使用第三方动画库，例如：[Animate.css](https://animate.style/)。

### 安装

使用 CDN：

```html
<link
  rel="stylesheet"
  href="https://cdnjs.cloudflare.com/ajax/libs/animate.css/3.7.2/animate.min.css"
/>
```

使用 npm：

```npm
npm i animate.css
```

导入

```js
// main.js
import 'animate.css'
```

现在，在 `transition` 元素中，我们可以使用 `enter-active-class` 和 `leave-active-class` 属性，将 `animate.js` 动画类添加到我们的元素上。

```html
<transition
  enter-active-class="animate__animated animate__fadeIn animate__zoomIn"
  leave-active-class="animate__animated animate__fadeOut animate__zoomOut"
>
  ...
</transition>
```

> **注意**：对于 `animate.js`，我们需要添加动画类 `animate__animated`。

它提供了许多动画类，详细[查看官网](https://animate.style/)。
