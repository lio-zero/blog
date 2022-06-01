# Vue 事件处理

[Vue 事件](https://vuejs.org/v2/guide/events.html)处理是每个 Vue 项目的必要方面。它用于捕获用户输入，共享数据以及许多其他创造性方式。

在本文中，我将介绍基础知识，并提供一些用于处理事件的代码示例。

## 基本事件处理

使用 `v-on` 指令（`@` 简称），我们可以监听 DOM 事件并运行处理程序方法或内联 Javascript：

```html
<div v-on:click="handleClick" />
<!-- 相当于 -->
<div @click="handleClick" />
```

我们将介绍您可能想捕获的一些更常见的事件，单击此处以获取 [DOM 事件](https://developer.mozilla.org/en-US/docs/Web/Events)的完整列表。

## 发出自定义事件

任何 Web 框架中的常见用例都是希望子组件能够向其父组件发出事件。这将允许双向数据绑定。

这样的一个示例是将数据从输入组件发送到父表单。

根据我们使用的是 Options API 还是 Composition API，发出事件的语法是不同的。

在 Options API 中，我们可以简单地调用 `this.$emit(eventName, payload)`：

```js
export default {
  methods: {
    handleUpdate() {
      this.$emit('update', 'Hello World')
    }
  }
}
```

但是，Composition API 没有 `this`。相反，我们可以使用 Vue3 `setup` 方法直接访问 `emit` 方法。

> `setup` 方法的第二个参数是上下文变量，它包含三个属性：`attrs`、`slot` 和 `emit`。

只要导入上下文对象，就可以使用与 Options API 相同的参数来调用 `emit`。

```js
export default {
  setup(props, context) {
    const handleUpdate = () => {
      context.emit('update', 'Hello World')
    }
    return { handleUpdate }
  }
}
```

整理代码的一种方法是使用对象解构直接导入 `emit`。看起来像这样。

```js
export default {
  setup(props, { emit }) {
    const handleUpdate = () => {
      emit('update', 'Hello World')
    }
    return { handleUpdate }
  }
}
```

无论我们使用 Options API 还是 Composition API，我们的父组件都以相同的方式监听自定义事件。

```html
<HelloWorld @update="inputUpdated" />
```

如果我们发出的方法也传递了一个值，则可以用两种不同的方式捕获它-取决于我们是内联工作还是使用其他方法。

首先，我们可以 `$event` 在模板中使用传递的值。

```html
<HelloWorld @update="inputUpdated($event)" />
```

其次，如果我们使用方法来处理事件，则传递的值将作为第一个参数自动传递给我们的方法。

```html
<HelloWorld @update="inputUpdated" />

<script>
  methods: {
    inputUpdated: (value) => {
      console.log(value) // WORKS TOO
    }
  }
</script>
```

## 处理鼠标修饰符

以下是我们可以在 `v-on` 指令中捕获的主要 [DOM 鼠标事件](https://developer.mozilla.org/en-US/docs/Web/API/MouseEvent) 的列表：

```html
<div
  @mousedown="handleEvent"
  @mouseup="handleEvent"
  @click="handleEvent"
  @dblclick="handleEvent"
  @mousemove="handleEvent"
  @mouseover="handleEvent"
  @mousewheel="handleEvent"
  @mouseout="handleEvent"
>
  与我互动！
</div>
```

对于我们的点击事件，我们还可以添加鼠标事件修饰符来限制哪些鼠标按钮将触发我们的事件。有三个鼠标按键：`left`、`right` 和 `middle`。

```html
<!-- 这将仅在鼠标左键单击时触发 -->
<div @mousedown.left="handleLeftClick">Left</div>
```

## 按键修饰符

我们可以监听三个 DOM [键盘事件](https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent)：

```html
<input
  type="text"
  placeholder="Type something"
  @keypress="handleKeyPressed"
  @keydown="handleKeyDown"
  @keyup="handleKeyUp"
/>
```

通常，我们希望在某个按键上监听这些事件，Vue 具有某些键的别名可以帮助到我们。

- `enter`
- `tab`
- `delete`（捕获“删除”和“退格”键）
- `esc`
- `space`
- `up`
- `down`
- `left`
- `right`

```html
<input type="text" placeholder="Type something" @keyup.enter="handleEnter" />
```

需要注意的是，Vue 2.x 中，我们还可以使用[键码](https://keycode.info/) 的方式，但在 Vue3.x 不在适用：

```html
<!-- ❌ -->
<input type="text" placeholder="Type something" @keyup.13="handleEnter" />
```

一些特殊的字符无法被匹配，如 "、'、/、=、> 和 .。这些应该在监听器内使用事件对象单独判断。

```html
<input type="text" placeholder="Type something" @keyup.,="handleEnter" />
```

> **注意**：Vue 3.x 不在支持自定义按键 `config.keyCodes`

## 系统修饰符

对于某些项目，我们可能只想在用户按下修饰符的情况下触发事件。修饰符类似于 `Command` 或 `shift`。

在 Vue 中，有四个系统修饰符。

- `shift`
- `alt`
- `ctrl`
- `meta` （在 Mac 上为 cmd，在 Windows 上为 Windows 键）

这对于在 Vue 应用程序中创建诸如自定义键盘快捷键之类的功能非常有用。

```html
<!-- 为 Shift + a clear 的自定义快捷方式 -->
<button @keyup.shift.a="clear">清除</button>
```

在 Vue 文档中，还有一个 `exact` 修饰符，确保仅在按下我们指定的键且没有其他键的情况下才触发事件。

```html
<!-- 仅为 Shift + a 执行 clear -->
<button @keyup.shift.a.exact="clear">清除</button>
```

鼠标按钮修饰符：`lefe`、`right` 和 `middle`，默认行为为 `left `（左键点击）。

```html
<button @click.middle="clear">清除</button>
```

## 事件修饰符

对于所有 DOM 事件，我们可以使用一些修饰符来更改其运行方式。无论是停止传播还是阻止默认操作，Vue 都有几个内置的 DOM 事件修饰符。

```html
<!-- 阻止默认操作 -->
<form @submit.prevent />

<!-- 停止事件传播 -->
<form @submit.stop="submitForm" />

<!-- 串联的修饰符 -->
<form @submit.stop.prevent="submitForm" />

<!-- 防止事件被触发多次 -->
<el-button @click.once="handleClose">执行一次</el-button>
```

使用修饰符时，顺序很重要；相应的代码会以同样的顺序产生。因此，用 `v-on:click.prevent.self` 会阻止所有的点击，而 `v-on:click.self.prevent` 只会阻止对元素自身的点击。

以下时 Vue 提供的完整事件修饰符：

- `stop` 阻止事件传播
- `prevent` 阻止默认事件
- `capture` 添加事件监听器时使用事件捕获模式，即内部元素触发的事件先在此处理，然后才交由内部元素进行处理
- `self` 只当在 event.target 是当前元素自身时触发处理函数，即事件不是从内部元素触发的
- `once` 仅执行一次
- `passive` 修饰符尤其能够提升移动端的性能

> **注意**：不要把 `.passive` 和 `.prevent` 一起使用，因为 `.prevent` 将会被忽略，同时浏览器可能会向你展示一个警告。请记住，`.passive` 会告诉浏览器你不想阻止事件的默认行为。

```html
<!-- 这其中包含 `event.preventDefault()` 的情况 -->
<div @scroll.passive="onScroll">...</div>
```

滚动事件的默认行为（即滚动行为）将会立即触发，而不会等待 `onScroll` 完成。
