# CSS scroll-behavior 属性 — 纯 CSS 平滑滚动

CSS 中的 `scroll-behavior` 属性是一个非常新的属性。定义要求 `scroll-behavior`（尤其是在选择锚链接时）具有平滑的过渡动画外观，而不是默认的、更刺眼的即时跳转。

```css
html {
  scroll-behavior: auto | smooth;
}
```

`scroll-behavior` 属性接受两个值，将切换打开和关闭的平滑滚动功能。

- `auto`（默认）：该值允许滚动框中的元素之间突然跳转。
- `smooth`：顾名思义，该值是滚动框内元素之间的平滑的过渡动画。

它是较新的属性，支持 Chrome 61+、Firefox 36+ 和 Opera 48+ 等主流浏览器。

![2022/2/5：scroll-behavior 支持情况](https://upload-images.jianshu.io/upload_images/18281896-4b757024f53d4502.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 示例：返回顶部

默认情况下，通过单击锚链接定位页面中的元素，浏览器将直接跳转到目标。

在不使用任何 JavaScript 的情况下，我们可以使用简单的 CSS `scroll-behavior` 属性平滑地滚动到给定的元素。

这里给出一个平滑滚动到顶部的示例，在 `<html>` 元素上提供以 ID 为目标的链接：

```html
<html id="top">
  <body>
    <a href="#top">跳转到页面顶部</a>
  </body>
</html>
```

将 `scroll-behavior` 属性设置为 `html` 元素可实现整个页面的平滑滚动效果：

```css
html {
  scroll-behavior: smooth;
}
```

> [查看效果](https://codepen.io/lio-zero/pen/qBRxNQL)

## 更多资料

- [scroll-behavior](https://css-tricks.com/almanac/properties/s/scroll-behavior/)
- [Downsides of Smooth Scrolling](https://css-tricks.com/downsides-of-smooth-scrolling/)
- [CSS overscroll-behavior 让滚动嵌套时父滚动不触发](https://www.zhangxinxu.com/wordpress/2020/01/css-overscroll-behavior/)
- [纯 CSS 平滑滚动“返回顶部”](https://moderncss.dev/pure-css-smooth-scroll-back-to-top/)
- [带有 Ken Kens 奖金效应的动画图片库字幕](https://moderncss.dev/animated-image-gallery-captions-with-bonus-ken-burns-effect/)
- [仅 CSS 可访问的下拉导航菜单](https://moderncss.dev/css-only-accessible-dropdown-navigation-menu/)
