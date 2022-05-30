# 为 contenteditable 元素添加占位符

假设我们想要为具有给定 `contenteditable` 属性的元素设置一个占位符：

```html
<div contenteditable></div>
```

> **注意**：`contenteditable` 用于指定元素内容是否可编辑，它可以应用于任何 HTML 元素，可以像 `input` 或 `<textare>` 那样工作编辑它们。

以下两种方法可以为 `contenteditable` 元素添加占位符。

## 使用 `:empty` 选择器

我们使用自定义属性 `data-placeholder` 来设置占位符：

```html
<div class="editable" contenteditable data-placeholder="Edit me"></div>
```

当元素值为空时，将显示该属性：

```css
.editable:empty:before {
  content: attr(data-placeholder);
}
```

## 处理事件

首先，我们将 `id` 和 `data-placeholder` 属性添加到元素中，如下所示：

```html
<div id="editMe" data-placeholder="Edit me" contenteditable></div>
```

当用户聚焦该元素时，如果它与占位符相同，我们将重置其内容。此外，当元素失去焦点时，如果用户不输入任何内容，其内容将被设置回占位符。

```js
const ele = document.getElementById('editMe')

// 获取占位符属性
const placeholder = ele.getAttribute('data-placeholder')

// 如果占位符为空，则将占位符设置为初始内容
ele.innerHTML === '' && (ele.innerHTML = placeholder)

ele.addEventListener('focus', function (e) {
  const value = e.target.innerHTML
  value === placeholder && (e.target.innerHTML = '')
})

ele.addEventListener('blur', function (e) {
  const value = e.target.innerHTML
  value === '' && (e.target.innerHTML = placeholder)
})
```

> [查看效果](https://codepen.io/lio-zero/pen/vYJJbMp)
