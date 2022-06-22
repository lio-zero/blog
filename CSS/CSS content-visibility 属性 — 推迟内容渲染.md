# CSS content-visibility 属性 — 推迟内容渲染

[`content-visibility`](https://web.dev/content-visibility/) 是新的 CSS 属性，可以提高页面加载性能。对于具有大量内容块、图像和视频丰富的复杂布局，解码数据和渲染像素可能是一项非常昂贵的操作，尤其是在低端设备上。

使用 `content visibility: auto`，我们可以在容器位于视口之外时提示浏览器跳过子对象的布局。

例如，您可以跳过在初始加载时渲染 `footer`：

```css
footer {
  content-visibility: auto;
  /* 1000px 是尚未渲染的部分的估计高度。 */
  contain-intrinsic-size: 1000px;
}
```

它是新的属性，仅支持 Chrome 85+、Edge 85+ 和 Opera 71+ 等少部分主流浏览器。

![2022/2/5：content-visibility 的支持情況](https://upload-images.jianshu.io/upload_images/18281896-ac3fe1a690335e52.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

如果您需要声明当前元素和它的内容尽可能的独立于 DOM 树的其他部分，可以使用 [CSS contain](https://developers.google.com/web/updates/2016/06/css-containment) 属性。这使得浏览器在重新计算布局、样式、绘图、大小或这四项的组合时，只影响到有限的 DOM 区域，而不是整个页面，可以有效改善性能。

```css
.ele {
  contain: none | strict | content | [ size | layout | style | paint ];
}
```

> **注意**：[`content-visibility: auto` 的行为类似于 `overflow: hidden;`](https://www.terluinwebdesign.nl/en/css/calculating-contain-intrinsic-size-for-content-visibility/)，但您可以通过应用 `padding-left/right` 来修复它，而不是使用默认的 `margin: 0 auto;` 以及声明的宽度。`padding` 基本上允许元素溢出 `content` 并进入 `padding`，而不会将盒模型作为一个整体离开并被切断。

另外，请记住，当新内容最终渲染时，您可能会引入一些 [CLS](https://web.dev/cls/)（Cumulative Layout Shift，累积布局偏移），因此最好使用具有适当大小占位符的 `contain-intrinsic-size`。

[一个关于 Content-visibility 的示例](https://codepen.io/vmpstr/pen/xxZoyMb)。

## 更多资源

- [content-visibility](https://css-tricks.com/almanac/properties/c/content-visibility/)
- [Calculating 'contain-intrinsic-size' for 'content-visibility'](https://www.terluinwebdesign.nl/en/css/calculating-contain-intrinsic-size-for-content-visibility/)
- [使用 CSS3 will-change 提高页面滚动、动画等渲染性能](https://www.zhangxinxu.com/wordpress/2015/11/css3-will-change-improve-paint/)
- [content-visibility——只需一行 CSS 代码，让长列表网页的渲染性能提升几倍以上！](https://juejin.cn/post/6908521872577527822#heading-1)
