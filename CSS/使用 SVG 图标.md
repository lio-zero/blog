# 使用 SVG 图标

没有理由不使用 SVG 图标：

```css
.logo {
  background: url('logo.svg');
}
```

SVG 在所有分辨率下都可以良好缩放，并且支持所有 [IE9](https://caniuse.com/#search=svg) 以后的浏览器，现在使用它替换您的 .png，.jpg，或 .gif 文件吧。

> **注意**： 针对仅有图标的按钮，如果 SVG 没有加载成功的话，以下样式对无障碍有所帮助：

```css
.no-svg .icon-only::after {
  content: attr(aria-label);
}
```
