# 使用 JavaScript 检查复选框是否被选中

假设你有这个复选框：

```html
<input type="checkbox" class="checkbox" />
```

想要检查复选框是否被选中，可以检查元素的 `checked` 属性：

```js
document.querySelector('.checkbox').checked
```

您还可以检查查找 `.checkbox:checked` 是否不返回 `null`：

```js
document.querySelector('.checkbox:checked') !== null
```

但我认为 `.checked` 更简洁。

不要使用 `getAttribute()` 查找 `checked` 属性值，因为如果复选框默认以这种方式被选中，那么它总是成立的：

```html
<input type="checkbox" checked />
```

也不要检查 `checkbox` 元素的 `value`。它始终处于启用状态，无论复选框是否选中。

## 进一步阅读

- 一篇不错的 checkbox 文章 — [The “Checkbox Hack” (and things you can do with it)](https://css-tricks.com/the-checkbox-hack/)
- 一个不错的自定义复选框 — [Custom checkbox](https://www.30secondsofcode.org/css/s/custom-checkbox)
