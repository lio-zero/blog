# BFC 及其应用

> **块格式化上下文（Block Formatting Context，BFC）** 是 Web 页面的可视化 CSS 渲染的一部分。它是布局过程中生成块级盒子的区域，也是浮动元素与其他元素的交互限定区域。

## BFC 特性

- BFC 在 Web 页面上是一个独立的容器，容器内外的元素互不影响
- 和标准文档流一样，BFC 内的两个相邻块级元素垂直方向的边距会发生重叠
- BFC 不会与浮动元素的盒子重叠
- BFC 在计算高度时会把浮动元素计算进去

## 触发 BFC

只要元素满足下面任一条件即可触发 BFC 特性（[详细](https://developer.mozilla.org/zh-CN/docs/Web/Guide/CSS/Block_formatting_context)）：

- 根元素（`<html>`）
- 浮动元素（元素的 `float` 不是 `none`）
- 绝对定位元素（元素的 position 为 absolute 或 fixed）
- `display 为 `inline-block`、`table-cells`、`flex`、`grid`...
- `overflow` 值不为 `visible` 的块元素（`hidden`、`auto`、`scroll`）
- ...

## BFC 应用

### 清除浮动

当父元素没有设置高度，且子元素为浮动元素的情况下，父元素会发生高度坍塌，上下边界重合，即浮动元素无法撑开父元素。

```html
<div class="parent">
  <div class="child"></div>
</div>
```

给子元素设置浮动：

```css
.child {
  float: left;
}
```

![高度塌陷](https://upload-images.jianshu.io/upload_images/18281896-ffb7fb17c3520e66.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

可以看到，子元素浮动后，父元素失去了高度。

**为了解决浮动元素造成的父元素高度塌陷问题，可以将父元素设置成一个 BFC 来清除浮动，将父元素整体设置为 BFC 环境。**

```css
.parent {
  overflow: auto;
}
```

![清除浮动](https://upload-images.jianshu.io/upload_images/18281896-0189447c6531425c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

[查看效果](https://codepen.io/lio-zero/pen/bGgzaqR)

### 元素浮动后发生重叠

```html
<div class="box1"></div>
<div class="box2"></div>
```

给第一个盒子设置浮动：

```css
.box1 {
  float: left;
}
```

![元素重叠](https://upload-images.jianshu.io/upload_images/18281896-48d1b6e1b66290bf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

可以看到，由于 `box1` 发生浮动，`box2` 未发生浮动，而浮动元素由于脱离文档流，第一个盒子堆叠在第二个盒子上。

为了让两个元素不重叠，我们把右边的盒子设置成 BFC：

```css
.box2 {
  overflow: hidden;
}
```

![元素重叠](https://upload-images.jianshu.io/upload_images/18281896-7b9a994b9b3b3629.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

[查看效果](https://codepen.io/lio-zero/pen/zYNePmj)

### 外边距重叠

在标准文档流中，毗邻的两个或多个块级元素之间垂直方向的 `margin` 会合并成一个 `margin`，会取两个元素 `margin` 最大的那一个，这就是外边距重叠。

有三种情况会形成[外边距重叠](https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_Box_Model/Mastering_margin_collapsing)（Margin collapsing）：

- **同一层相邻元素之间**
- **没有内容将父元素和后代元素分开**
- **空的块级元素**

而创建了块级格式化上下文（BFC）的元素，不会和它的子元素发生 `margin` 重叠。

我们可以使用例如 `overflow：hidden` 来产生一个 BFC 环境来解决该问题。

```html
<div></div>
<div></div>
```

给两个 `<div>` 元素设置外边距：

```css
div {
  margin: 50px;
}
```

![外边距重叠](https://upload-images.jianshu.io/upload_images/18281896-054cc08fc4e02d92.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

可以看到，第一个元素的下边距和第二个元素的上边距发生了重叠。

**解决边距重叠的问题，可以将其放在不同的 BFC 容器中。**

```html
<div class="container">
  <div></div>
</div>

<div class="container">
  <div></div>
</div>
```

![外边距重叠](https://upload-images.jianshu.io/upload_images/18281896-38aad629ee81ee4d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

因为 BFC 是一个独立的容器，容器内外互不影响。

[查看效果](https://codepen.io/lio-zero/pen/QWdYapO)
