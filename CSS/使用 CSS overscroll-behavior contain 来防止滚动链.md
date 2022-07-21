# 使用 CSS overscroll-behavior: contain 来防止滚动链

当用户滚动嵌入的网页（或网页上的任何嵌套滚动容器）并到达末尾时，浏览器将开始滚动外部页面。这被称为[滚动链](https://drafts.csswg.org/css-overscroll-behavior/#scroll-chaining)。

> 滚动链是指滚动从一个滚动容器传播到滚动链后面的祖先滚动容器。

一些用户可能会觉得这种行为很烦人。幸运的是，可以通过将 `overscroll-behavior: contain` 应用于嵌入页面（或滚动容器）的 `<body>` 元素来禁用滚动链接。。

```css
body {
  overscroll-behavior: contain;
}
```

Firefox、Chromium 和 Edge 通过 **[autoprefixer](https://github.com/postcss/autoprefixer)** 支持此功能，但 Safari 目前还不支持此功能：

![image.png](https://upload-images.jianshu.io/upload_images/18281896-671fb079086cac7a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

> [查看效果](https://codepen.io/lio-zero/pen/LYeLZqw)
