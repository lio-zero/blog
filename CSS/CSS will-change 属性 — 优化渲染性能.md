# CSS will-change 属性 — 优化渲染性能

> **[`will-change`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/will-change) 属性可以开启 GPU 硬件加速以提升/优化网站动画渲染性能。**

`will-change` 通过告知浏览器该元素会有哪些变化，使浏览器提前做好优化准备，增强页面渲染性能。

语法：

```css
will-change: <animateable-feature> = scroll-position | contents | <custom-ident>;
```

`will-change` 属性的取值：

- `auto` — 应用标准浏览器优化。
- `scroll-position` — 表示元素的滚动位置将在不久的将来某个时候设置动画，以便浏览器准备在元素的滚动窗口中看不到的内容。
- `contents` — 表示希望在不久后改变元素内容中的某些东西，或者使它们产生动画。
- `<custom>` — 任何用户定义的属性，比如 `transform` 或 `opacity`，表示希望在不久后改变指定的属性名或者使之产生动画，

> **注意**：不要将 `will-change` 应用到太多元素上，如果过度使用的话，可能导致页面响应缓慢或消耗大量资源。详细内容查看 [will-change](https://developer.mozilla.org/zh-CN/docs/Web/CSS/will-change)。

以下是 `will-change` 的支持情況：

![2022/2/5：will-change 支持情況](https://upload-images.jianshu.io/upload_images/18281896-bcaa354083f45bef.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

[一个关于 will-change 的示例](https://codepen.io/nchan0154/pen/abVNYNo)

## 更多资源

- [will-change](https://css-tricks.com/almanac/properties/w/will-change/)
- [Fix scrolling performance with CSS will-change property](https://www.fourkitchens.com/blog/article/fix-scrolling-performance-css-will-change-property/)
- [My white whale: A use case for will-change](https://www.nicchan.me/blog/a-use-case-for-will-change/)
- [Everything You Need to Know About the CSS will-change Property](https://dev.opera.com/articles/css-will-change-property/)
- [An Introduction to the CSS will-change Property](https://www.sitepoint.com/introduction-css-will-change-property/)
- [使用 CSS3 will-change 提高页面滚动、动画等渲染性能](https://www.zhangxinxu.com/wordpress/2015/11/css3-will-change-improve-paint/)
- [How to create high-performance CSS animations](https://web.dev/animations-guide/)
