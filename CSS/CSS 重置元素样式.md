# CSS 重置元素样式

## CSS 重置

CSS 重置有助于在不同浏览器之间强制实现样式一致性，并为样式元素提供了一个干净的列表。您可以使用诸如 [Normalize](http://necolas.github.io/normalize.css/) 等 CSS 重置库，也可以使用更简化的重置方法：

```css
*,
*::before,
*::after {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
```

现在，元素将被去除 `margin` 和 `padding`，`box-sizing` 允许您使用 CSS 盒模型管理布局。

**注意**：如果遵循下面的[继承 `box-sizing`](https://github.com/AllThingsSmitty/css-protips#inherit-box-sizing) 提示，您可能会选择在 CSS 重置中不包含 `box-sizing` 属性。

## 继承 `box-sizing`

从 `html` 元素继承 `box-sizing`：

```css
html {
  box-sizing: border-box;
}

*,
*::before,
*::after {
  box-sizing: inherit;
}
```

这使得在插件或其它组件中更改 `box-sizing` 变得更容易。

## 使用 unset 而不是重置所有属性

重置元素的属性时，不需要重置每个单独的属性：

```css
button {
  background: none;
  border: none;
  color: inherit;
  font: inherit;
  outline: none;
  padding: 0;
}
```

你可以用 `all` 简写來指定所有元素的属性。 将该值设置为 `unset` 会将元素的属性更改为其初始值：

```css
button {
  all: unset;
}
```

> **注意**： IE11 不支持 `all` 和 `unset` 的简写。
