# display-none 与 [hidden] 的区别

HTML5 添加了 `hidden` 属性，该属性与 CSS `display: none` 具有相同的效果。如果使用 `hidden` 属性，则无论其值如何，都不会显示元素：

```html
<div hidden>
  <!-- 这将从屏幕上消失 -->
</div>
```

两者都会对屏幕阅读器隐藏内容。

## 区别

- `display: none` 和 `hidden` 属性的工作方式相同。但是 `hidden` 属性提供了更好的语义。
- `display: none` 在旧浏览器中有效，但 IE 10 及更低版本原生不支持 `hidden`。

为了解决这个问题，我们可以简单地设置：

```css
[hidden] {
  display: none;
}
```

它包含在现代 CSS 规范化库中，比如 [Normalize.css](https://necolas.github.io/normalize.css)。
