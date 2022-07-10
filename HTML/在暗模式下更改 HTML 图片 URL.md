# 在暗模式下更改 HTML 图片 URL

如果系统处于暗模式，使用 CSS 很容易应用更改，使用 `prefers-color-scheme` 媒体功能。

但如何更改 HTML 中定义的图像，而不是 CSS 规则？

事实证明，有一种简单的 HTML 方法可以做到这一点，而无需涉及任何 CSS 或 JavaScript。

我们可以使用 `picture` 标签来包装 `img` 标签：

```html
<picture>
  <source srcset="dark.png" media="(prefers-color-scheme: dark)" />
  <img src="light.png" />
</picture>
```

如果支持并启用暗模式，则 `dark.png` 图像将用作 `img` 标签的源。

该标签得到很好的支持，而不支持它或不支持暗模式的旧浏览器将退回到显示 `light.png` 图像。

需要注意的是，在任何情况下，浏览器都不会下载 2 张图片：如果是暗模式，在这个例子中，它只会下载 `dark.png` 图像，如果是亮模式，它只会下载 `light.png`，因此不会浪费带宽。
