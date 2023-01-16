# label 标签触发相关 input 的状态

`label` 标签可以通过 `for` 属性和 `input` 的 `id` 值关联起来来触发相关 `input` 的状态。当用户点击 `label` 标签时，会触发相关联的 `input` 元素的焦点，并将光标移动到该元素上。

`input` 元素的其他伪类，如 `:hover` 和 `:active` 也是如此，当用户在 `label` 标签上悬停时将被触发。

```html
<label for="name">Name: </label>
<input id="name" />
```

```css
#name:hover {
  background: yellow;
}
```

在上面的例子中，当你将鼠标悬停在 `label` 标签上时，输入框将显示为黄色。

`label` 标签还可以通过包含在 `input` 元素内来触发相关 `input` 的状态。这样，当用户点击 `label` 标签时，会触发相关联的 `input` 元素的焦点。

```html
<label for="foo">
  Name:
  <input id="foo" />
</label>
```

另外，`label` 标签还可以通过点击 `label` 标签来更改相关联的 `input` 的状态。如对于复选框和单选按钮，点击 `label` 标签可以切换选中/未选中状态。这样做可以方便用户的操作，提高表单的可用性。

> **注意**：当你单击一个与 `label` 标签相关的表单元素时，表单元素上的 `click` 事件将会被触发。
