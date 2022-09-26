# CSS 盒模型

CSS 是关于盒子的。页面上显示的所有内容都有一个框，[**盒模型**（box model）](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/Building_blocks/The_box_model)描述了如何计算该框的大小 —— 考虑了边距（`margin`）、边框（`border`）、填充（`padding`）和内容（`content`）。

以下图片说明了盒模型（Box Model）的层级分部：

![CSS 盒模型](https://upload-images.jianshu.io/upload_images/18281896-1d9164962395e977.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

由内到外，分别是：

- **`content`（内容）** — 盒子的内容，其中显示文本和图像
- **`padding`（填充）** — 围绕内容的透明区域（即边框和内容之间的空间）
- **`border`（边框）** — 围绕在内边距和内容外的边框。
- **`margin`（边界）** — 边距周围的透明区域（即边距与任何相邻元素之间的空间）

它们解释了元素的大小，对应着相应的 CSS 属性，除了 content（CSS 属性 `width/height`）。

## IE 盒模型和 W3C 盒模型

这里需要介绍一下 IE 盒模型和 W3C 盒模型的区别。

盒模型分为两类：**W3C 标准盒模型和 IE 盒模型**。

- W3C 标准盒模型 — 元素的宽度（`width`）和高度（`height`）只包含内容（`content`）。而 `width`、`padding` 和 `border` 一起决定了整个盒子的大小。
- IE 盒模型 — 元素的宽度（`width`）和高度（`height`）包含了 `content`、`padding` 和 `border`。整个盒子的大小由 `width/height` 决定。

```css
.box {
  width: 200px;
  padding: 10px;
  border: 5px solid plum;
  margin: 0;
}
```

以上元素的宽度在 IE 盒模型和 W3C 盒模型中是不同的：

- W3C 盒模型 — `200px + 10px + 10px + 5px + 5px = 230px`。
- IE 盒模型 — `200px`

标准 CSS 盒模型采用给定元素的宽度，然后在该宽度上添加 `padding` 和 `border`，这意味着元素占用的空间大于给定的宽度。

## 如何进行不同盒模型的切换？

CSS3 `box-sizing` 属性提供给我们切换盒模型的权利，它有如下三个值：

- `content-box` — 默认的标准（W3C）盒模型元素效果
- `border-box` — 触发怪异（IE）盒模型元素的效果
- `inherit` — 继承父元素 `box-sizing` 属性的值

```css
.box {
  box-sizing: content-box | border-box | inherit;
}
```

> **注意**：到 IE8+ 才支持使用 `box-sizing` 进行盒模型切换。

我们可能不明确或者总是去计算加上 `padding` 和 `border` 才能获取元素的实际大小，这变的很麻烦。这时，我们可以使用 `box-sizing: border-box` 在设置有 `padding` 和 `border` 值时不把元素的宽度/高度撑开。

## 更多资料

- [MDN Docs：盒模型](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/Building_blocks/The_box_model) — 完整的介绍
- [CSS Box Model Module Level 3](https://www.w3.org/TR/css-box-3/)
- [BFC 及其应用](https://github.com/lio-zero/blog/blob/main/CSS/BFC%20%E5%8F%8A%E5%85%B6%E5%BA%94%E7%94%A8.md)
- [CSS 盒模型之内边距、边框、外边距 十九问 （持续更新）](https://juejin.cn/post/6880111680153059341)
