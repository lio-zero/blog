# style="width: ___" 与 width="___"


有两种方法可以定义图片的尺寸：

- 使用 `height` 或 `width` 属性：

```html
<img height="100" />
```

- 或者在 CSS 样式中使用 `height` 或 `width` 属性：

```html
<img style="height: 100px" />
```

## 区别

CSS `width` 和 `height` 属性可用于所有 HTML 元素。但 HTML `width` 和 `height` 属性仅适用于某些元素，如 `canvas`、`img`、`table`、`td` 等。

```html
<!-- 工作 -->
<img width="200px" />

<!-- 不起作用 -->
<div width="200px"></div>
```

对于 `canvas` 元素，它们不会产生相同的结果。

根据 [HTML 规范](https://html.spec.whatwg.org/multipage/canvas.html#attr-canvas-width)，如果缺少 `width` 和 `height` 属性，`canvas` 将使用默认值。

`width` 属性默认为 300，`height` 属性默认为 150。

建议直接或通过 JavaScript 设置 `canvas` 的 `height` 和 `width` 属性，以避免 `canvas` 拉伸的问题。

```html
<!-- 工作 -->
<canvas height="100" width="100">
  <!-- 不起作用 -->
  <canvas style="height: 100px; width: 100px;"></canvas>
</canvas>
```

`canvas` 的 `width` 和 `height` 属性必须是不带单位的正数。`width="100px"` 将不起作用，尽管它似乎是其他元素的有效属性声明。

CSS 样式属性的优先级高于 HTML 属性。

在以下示例中，`height: 200px` 属性将覆盖 `height="100px"` 属性：

```html
<!-- 图像的宽度为 200px -->
<img height="100px" style="height: 200px" />
```

`width` 和 `height` 属性还广泛用于电子邮件中，我们必须支持多种屏幕大小（移动、桌面）和各种电子邮件客户端。
