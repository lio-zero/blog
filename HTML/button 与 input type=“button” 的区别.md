# button 与 input type="button" 的区别

- `<button>` 可以包含 HTML。`<input type="button" />` 是空元素（如 `br`、`hr`、`image`），因此不能包含内容。
- `<button>` 支持伪元素，例如 `::after` 和 `::before`，这对于设置按钮样式非常有用。而 `<input type="button" />` 没有。
- 默认情况下，`<button>` 具有默认属性 `type="submit"`。这意味着，如果没有指定 `type` 属性，单击该按钮将提交其封闭表单。

如果您希望 `input` 作为提交按钮，我们必须将 `type` 属性更改为 `submit`。

## 建议

- `button` 元素比 `button` 类型的 `input` 更具语义。如果要创建可单击的按钮，建议使用 `button` 元素。
- 始终指定 `button` 元素的 `type` 属性。可能的值是

| 值       | 描述                             |
| -------- | -------------------------------- |
| `submit` | 按钮将表单数据提交到服务器       |
| `reset`  | 将表单输入重置为初始值           |
| `button` | 默认情况下，按下时不执行任何操作 |
