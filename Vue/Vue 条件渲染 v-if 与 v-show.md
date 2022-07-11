# Vue 条件渲染 v-if 与 v-show

## 什么是条件渲染？

在 Vue 中，有两种方法可以有条件地渲染应用程序的某些部分：`v-if` 和 `v-show`。

条件渲染是控制是否渲染模板代码的能力。我们可以使用应用程序的当前状态来实现这一点。

让我们看一个例子。假设我们正在创建一个表单，如果密码长度小于 6 个字符，我们希望显示一条无效的输入错误消息。

```html
<tempalte>
  <div>
    {{ email }} {{ password }}
    <p><input type="text" placeholder="Email" v-model="email" /></p>
    <p><input type="pass" placeholder="pass" v-model="pass" /></p>
    <p class="error-message" v-if="pass.length < 6">密码必须至少为6个字符</p>
  </div>
</template>
<script setup>
import { ref } from 'vue'

const email = ref('')
const password= ref('')
</script>
```

在本例中，我们使用 `v-if` 指令，然后传入一个布尔语句，如果语句为 `true`，渲染元素。如果为 `false`，则不会渲染。

回到例子，我们将 `v-if` 替换为 `v-show`：

```html
<p class="error-message" v-show="pass.length < 6">密码必须至少为6个字符</p>
```

可以看到，它们有同样效果，但它们的工作方式是不同的。

## `v-if` 和 `v-show` 有什么区别？

**关键区别在于 `v-if` 有条件地渲染元素，而 `v-show` 有条件地显示元素。**

这意味着 `v-if` 在切换条件时实际上会销毁和重新创建元素。同时，`v-show` 将始终将元素保留在 DOM 中，并且只会通过更改其 CSS 来切换其显示。

```html
<template>
  <div>
    <p v-if="active">使用 v-if</p>
    <p v-show="active">使用 v-show</p>
    <button @click="active = !active">切换状态</button>
  </div>
</template>
<script setup>
  import { ref } from 'vue'

  const active = ref(false)
</script>
```

当我们的元素被显示时，DOM 看起来像这样。

![active: true](https://upload-images.jianshu.io/upload_images/18281896-102255b0ae3fff4a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

当我们按下按钮时，DOM 看起来像这样。

![image.png](https://upload-images.jianshu.io/upload_images/18281896-b6a819f6aac25bcf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

可以看到，使用 `v-if`，条件块已从 DOM 中完全删除。但是对于 `v-show`，它 `display` 被设置为 `none`。

## 如何选择？

> 经常有这么一道面试题，问 `v-if` 与 `v-show` 的区别？

回顾上文，我们知道两者的共同点都是**控制元素显示和隐藏**。

那他们的不同点呢？这里总结几点：

- `v-show` 控制的是元素的 CSS（display）；`v-if` 是控制元素本身的添加或删除。
- `v-show` 切换的时候不会触发组件的生命周期。 `v-if` 由 `false` 变为 `true` 会触发组件的`beforeCreate`、`create`、`beforeMount`、`mounted` 钩子（跟组件的初始化执行的钩子一致），由 `true` 变为 `false` 会触发组件的 `beforeDestory`、`destoryed` 钩子。

如果以性能为准则：

- `v-if` 具有更高的切换成本（每当条件更改时）
- `v-show` 具有更高的初始渲染成本。

因此，如果您需要频繁切换某些内容，请使用 `v-show`。如果条件在运行时变化不那么频繁，请使用 `v-if`。

另一件要考虑的事情是使用 `v-if` 允许我们将其他逻辑块与它结合使用。具体来说，我们可以使用 `v-else` 和 `v-else-if` 块来真正将高级逻辑构建到我们的应用程序中。

```html
<template>
  <p v-if="active">使用 v-if</p>
  <p v-else-if="true">else if</p>
  <p v-else>else</p>
</template>
```
