# 使用 JavaScript 禁用按钮

HTML 按钮是少数具有自己状态的元素之一，以及几乎所有的表单控件。

一个常见的需求是使用 JavaScript 以编程方式禁用/启用按钮。

例如，您只想在填充文本输入元素时启用按钮。

或者当点击一个特定的复选框时，比如你看到的那些说**我阅读了条款和条件**，实际上没有人阅读的东西。

这是如何做到的。

选择元素：

```js
const button = document.querySelector('button')
```

如果有多个按钮，则可能需要使用 `document.querySelectorAll()` 并循环遍历结果。

无论如何，一旦有了元素引用，就可以将其 `disabled` 属性设置为 `true` 以禁用它：

```js
button.disabled = true
```

要再次启用它，请将其设置为 `false` 以再次启用：

```js
button.disabled = false
```
