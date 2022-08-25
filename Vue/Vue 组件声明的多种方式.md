# Vue 组件声明的多种方式

- [单文件组件](https://vuejs.org/guide/scaling-up/sfc.html)（SFC）

```html
<script setup>
  import { ref } from 'vue'
  const hi = ref('Hello Vue!')
</script>

<template>
  <p class="hi">{{ hi }}</p>
</template>

<style>
  .hi {
    color: plum;
    font-weight: bold;
  }
</style>
```

- [渲染函数](https://vuejs.org/guide/extras/render-function.html) — 创建虚拟 DOM 节点（vnode），Vue 2 和 Vue 3 略有不同。

```js
import { h } from 'vue'

const vnode = h('button', 'Hello World!')
```

> 您可以在 [Vue SFC Playground](https://sfc.vuejs.org/#eNptULtuwzAM/BVWi10gkdHVcAJ061igQxctedCPQBIFiU4LOP73UnZadMjGI3l3PE7qNQR9HVHVqkmnOASGhDyGvfGDCxQZJojYwgxtJAeFrBbGn8gnhhApJNjBGdvB43tG5WQ8AOM31/DBcfCd8fPzL8GlTtZFrize0FqCT4r2/FTIwj+7/qHZ1dMZhd2XxXFkJl9s1gN0doPbLavfnciittSVC0d6TbVGk1ACGF2wB0ZBAE3/sp+m5bJ5bipBS3fwYRTPrRMBuzNK5kbJqKn+2Gqj1pO37hD0JZGXHy7xzX2QjKph6eSehMnYqJ45pLqqUnvKn78kTbGrpNJx9Dw41Jjc9hjpK2EUYaOyxCyPVPMPiQ6W0w==) 进行平常的 Demo 演示、练习。

- [JSX/TSX](https://vuejs.org/guide/extras/render-function.html#jsx-tsx) — 如果您使用 Vite 构建项目，可以使用 [@vitejs/plugin-vue-jsx](https://github.com/vitejs/vite/tree/main/packages/plugin-vue-jsx) 插件以支持它。

```js
const elem = <h1>我喜欢使用 JSX!</h1>
```

> 关于 JSX 的介绍：[JSX Introduction](https://facebook.github.io/jsx/#sec-intro)。

## 更多资料

还有其他一些类似形式创建组件的方法，这里不再介绍，详细内容可以阅读下文：

- [7 Ways To Define A Component Template in VueJS](https://medium.com/js-dojo/7-ways-to-define-a-component-template-in-vuejs-c04e0c72900d)
- [Writing multiple Vue components in a single file](https://codewithhugo.com/writing-multiple-vue-components-in-a-single-file/)
