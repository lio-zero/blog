# 浏览器 Hack

浏览器 Hack 表示各个浏览器下的兼容性问题。

由于不同的浏览器和浏览器各版本对 CSS 的支持及解析结果不一样，以及 CSS 优先级对浏览器展现效果的影响，我们可以据此针对不同的浏览器情景来应用不同的 CSS。同样，包括 JavaScript，不同的浏览器也有一些 Hack，这里我们举例一些 CSS 示例。

## 仅限 Mozilla

```css
@-moz-document url-prefix() {
  .box {
    color: blue;
  }
}
```

## 仅限 WebKit

```css
@media all and (-webkit-min-device-pixel-ratio: 1) {}
```

## 仅限 IE

IE6 能识别下划线 `_` 和星号 `*`。

```css
.box {
  *background: plum; /* IE6 */
}
```

IE7 能识别星号 `*`，但不能识别下划线 `_`。

```css
.box {
  _background: plum; /* IE6 */
}
```

条件注释，只在 IE 上有效

```html
<!--[if IE 6]> Internet Explorer 6 <![endif]-->
<!--[if IE 7]> Internet Explorer 7 <![endif]-->
<!--[if IE 8]> Internet Explorer 8 <![endif]-->
<!--[if IE 9]> Internet Explorer 9 <![endif]-->

<!--[if lte IE 6]> Internet Explorer 6 or less <![endif]-->
<!--[if lte IE 7]> Internet Explorer 7 or less <![endif]-->
<!--[if lte IE 8]> Internet Explorer 8 or less <![endif]-->
<!--[if lte IE 9]> Internet Explorer 9 or less <![endif]-->

<!--[if gte IE 6]> Internet Explorer 6 or greater <![endif]-->
<!--[if gte IE 7]> Internet Explorer 7 or greater <![endif]-->
<!--[if gte IE 8]> Internet Explorer 8 or greater <![endif]-->
<!--[if gte IE 9]> Internet Explorer 9 or greater <![endif]-->
```

## 清除浮动 hack

```css
/**
 * For modern browsers
 * 1. The space content is one way to avoid an Opera bug when the
 *    contenteditable attribute is included anywhere else in the document.
 *    Otherwise it causes space to appear at the top and bottom of elements
 *    that are clearfixed.
 * 2. The use of `table` rather than `block` is only necessary if using
 *    `:before` to contain the top-margins of child elements.
 */
.cf:before,
.cf:after {
  content: ' '; /* 1 */
  display: table; /* 2 */
}

.cf:after {
  clear: both;
}

/**
 * For IE 6/7 only
 * Include this rule to trigger hasLayout and contain floats.
 */
.cf {
  *zoom: 1;
}
```

以上代码来自 [A new micro clearfix hack](http://nicolasgallagher.com/micro-clearfix-hack/)，其对清除浮动做出了简单的解释。

## 更多

- [BROWSERHACKS](http://browserhacks.com/) 网站列出了来自整个互联网特定于**浏览器的 CSS 和 JavaScript hack**。
- [Can I Use](https://www.caniuse.com/) 可以查看属性的兼容情况
