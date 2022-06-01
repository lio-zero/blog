# Vue Computed — 计算属性

![计算属性](https://upload-images.jianshu.io/upload_images/18281896-ee4ce0c597e7c38e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在模板内使用表达式非常便利，但是设计它们的初衷是用于简单运算的。在模板中放入太多的逻辑会让模板过重且难以维护。

假设我们要呈现一个使用连字符 `-` 连接各个单词的字符串，并且全部小写，它将看起来像这样：

```html
<span> {{ text.replace(/\s/g, '-').toLowerCase() }} </span>
```

这会不会让你看起来有点杂乱，且当需要在更多的模板中使用此操作，它将更加难以处理。

这时 Vue [计算属性](https://v3.cn.vuejs.org/guide/computed.html#%E8%AE%A1%E7%AE%97%E5%B1%9E%E6%80%A7)就派上用场了，对于任何复杂逻辑，你都应当使用**计算属性**。

## 简单示例

```js
export default {
  data() {
    return {
      text: 'Front End Interview'
    }
  },
  computed: {
    handleText() {
      return this.text.replace(/\s/g, '-').toLowerCase()
    }
  }
}
```

应用到模板中，它将更加清晰易读的方式进行渲染。

```html
<span> {{ handleText }} </span>
```

## Vue 计算属性缓存

由于计算属性类似于 [Vue Watch](https://cn.vuejs.org/v2/guide/computed.html#%E4%BE%A6%E5%90%AC%E5%99%A8)，因此它将观察一个响应式数据，并在该响应式数据发生更改时进行更新。

> **计算属性是基于它们的响应式依赖进行缓存的**。它只在相关响应式依赖发生改变时它们才会重新求值。

对于我们的示例，它只会在文本更改时重新计算。否则，它将返回上次更改的缓存值。

下面来自 [Vue 文档](https://vuejs.org/v2/guide/computed.html) 中的示例，显示了一个永远不会重新计算的计算属性。

```js
computed: {
  now() {
    return new Date()
  }
}
```

尽管计算属性现在返回的值实际上一直在变化，但是 Vue 不会监视任何依赖。因此，它永远不会被重新计算。

你可以始终使用常规的 Vue 方法，该方法将在每个 render 上重新计算一次。

```js
methods: {
  now() {
    return new Date()
  }
}
```

## 计算属性的 setter

默认情况下，计算属性是只读的，无法设置。但是，如果要为允许设置其依赖的计算属性添加钩子。你可以这么做：

```js
// 带 setter 的计算属性
computed: {
  handleText: {
    get() {
      return this.text.replace(/\s/g, '-').toLowerCase()
    },
    set(value) {
      this.text = value
    }
  }
}
```

例如，以下是一些命令及其对代码的影响。

```js
console.log(this.handleText) // "front-end-interview-questions"
this.handleText = 'Algorithmic Interview Questions'
console.log(this.text) // "Algorithmic Interview Questions"
console.log(this.handleText) // "algorithmic-interview-questions"
```

[查看效果](https://codepen.io/lio-zero/pen/poRBOZW)

## Vue3 Composition API 中的计算属性

使用 `Vue3 Composition API` 访问计算属性的方式略有不同。需要先导入 `computed`：

```js
import { ref, computed, onMounted } from 'vue'
```

导入后，我们可以在 `setup` 方法中使用计算属性。

```html
<script setup>
  // Composition API 中的计算属性
  import { ref, computed, onMounted } from 'vue'
  const text = ref('Front End Interview')
  const handleText = computed({
    get() => text.value.replace(/\s/g, '-').toLowerCase(),
    set(value) => (text.value = value)
  })

  onMounted(() => {
    console.log(handleText.value) // "front-end-interview"
    handleText.value = 'Algorithmic Interview Questions'
    console.log(text.value) // "Algorithmic Interview Questions"
    console.log(handleText.value) // "algorithmic-interview-questions"
  })
</script>
```

可以看到，`computed` 属性的代码本身基本相同，但是设置不同。

[查看效果](https://codepen.io/lio-zero/pen/RwKOYXW)
