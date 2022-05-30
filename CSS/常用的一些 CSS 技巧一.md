# 常用的一些 CSS 技巧一

你可以看看其他常用的 CSS 技巧：

- [常用的一些 CSS 技巧二 — 选择器（伪类与伪元素）](https://www.jianshu.com/p/0be9e3dc04c0)
- [常用的一些 CSS 技巧三](https://www.jianshu.com/p/4a99b2b36465)

## CSS3 中的 counter

- 使用 `counter` 属性可将任何元素转换为编号列表。类似于有序列表标签的工作方式 `<ol>`
  - 定义并初始化计数器 `counter-reset: tidbit-counter 19;`
  - 增量计数器 `counter-increment: <counter name> <integer>`
  - 使用 `counter(<counter name>, <counter list style>)` 作为 `content` 的内容
  - 查看 [counter list style](https://developer.mozilla.org/en-US/docs/Web/CSS/list-style-type#Values) 样式的完整列表

```html
<div>
  <h2>HTML</h2>
  <h2>CSS</h2>
  <h2>JS</h2>
</div>
```

CSS 如下：

```css
div {
  counter-reset: tidbit-counter 19;
}

h2::before {
  counter-increment: tidbit-counter;
  content: counter(tidbit-counter, thai);
}
```

效果如下：

![counter](https://upload-images.jianshu.io/upload_images/18281896-814fd949fa889d66.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 使用 CSS white-space 修复文本重叠

有时 `nowrap` 会导致文本重叠

```css
.container {
  display: flex;
}

div {
  white-space: nowrap;
}
```

![nowrap](https://upload-images.jianshu.io/upload_images/18281896-00a64d79204be949.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们设置了 `nowrap` 并没有发送重叠的问题，但当我们给其添加 width 的时候，看看会发生什么情况

```css
div {
  width: 100px;
}
```

![nowrap](https://upload-images.jianshu.io/upload_images/18281896-0e59d3158e83aafa.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

发生这种情况是因为原本的 `div` 宽度的默认值为 `width: auto`，这意味着盒子会根据其内容展开。但是，当我们给其一个固定的宽度时，会限制它的增长。文本无处可去，流进了同级框中。

我们可以通过将设置 `white-space` 为 `normal` 轻松修复它

```css
div {
  white-space: normal;
}
```

![normal](https://upload-images.jianshu.io/upload_images/18281896-6ac0f59622699d87.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## Flexbox 居中元素

使用 CSS Flexbox，能够水平和垂直居中放置某些东西非常简单。标准方法是使用 flex 属性。但是您也可以使用 `margin` 来做。

使用 flex 属性：

```css
.parent {
  display: flex;
  align-items: center;
  justify-content: center;
}
```

使用具有自动 `margin` 边距的 Flexbox：

```css
.parent {
  display: flex;
}

.child {
  margin: auto;
}
```

效果如下：

![flex 水平垂直居中](https://upload-images.jianshu.io/upload_images/18281896-6c6a463d57bcd501.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## Gird 居中元素

```css
.parent {
  display: grid;
}

.child {
  /* place-items: center center; */
  justify-self: center;
  align-self: center;
}
```

`grid` 和 `margin`

```css
.parent {
  display: grid;
}

.child {
  margin: auto;
}
```

## 在 CSS 中更改光标颜色

使用 `caret-color` 更改光标（尖号）的颜色：

```css
input {
  /* default */
  caret-color: auto;
}

input {
  /* custom */
  caret-color: DeepPink;
}
```

![caret-color](https://upload-images.jianshu.io/upload_images/18281896-1d6c682dd8baf4bd.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 使非密码输入使用符号替代

这适用于文本输入（例如 `text`、`textarea` 等），但不能更改实际的密码输入。

`password` 默认样式

```css
input {
  -webkit-text-security: disc; /* default */ }
}
```

![disc](https://upload-images.jianshu.io/upload_images/18281896-ca89962fb2fe2043.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```css
input {
  -webkit-text-security: none;
}
input {
  -webkit-text-security: circle;
}
input {
  -webkit-text-security: square;
}
```

![circle](https://upload-images.jianshu.io/upload_images/18281896-eb001906c79276a1.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![square](https://upload-images.jianshu.io/upload_images/18281896-f4c1672a26c46aae.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## CSS user-select 禁用文本选择

- 使用 `user-select: none` 使元素的内容不可选。
- 使用 `user-select: all` 用户只需点击一次选择文本

如果您试图创建一个简单的文本复制和粘贴体验，这将非常有用 👍

```css
/* 只需单击一次即可选择所有文本 */
p {
  user-select: all;
}

/* 禁用文本选择 */
p {
  user-select: none;
}
```

## 自定义 CSS 选择样式

CSS `::selection` 伪元素使您可以在文本突出显示时将样式应用于文本。

```css
p::selection {
  background: DeepPink;
  color: white;
}
```

对于 Firefox，您将需要使用 `::-moz-selection`

```css
p::-moz-selection {
  background: DeepPink;
  color: white;
}
```

## CSS 变量

原生 CSS 支持变量，而无需 sass 预处理器，也无需编译

你可以在全局范围的任何地方访问变量，而本地作用域将只在特定的选择器中

### 全局范围内定义

```css
/* 所有选择器中都可用 */
:root {
  --color-red: red;
}

.text {
  color: var(--color-red);
}

.text-1 {
  color: var(--color-red);
}
```

### 局部范围内定义

```css
/* 仅限于.local 类 */
.local {
  --color-blue: blue;
  color: var(--color); /* blue */
}

.text {
  color: var(--color-blue); /* 没有效果，找不到改变量 */
}
```

## text-rendering

CSS 关于文本渲染的 [`text-rendering`](https://developer.mozilla.org/en-US/docs/Web/CSS/text-rendering) 属性告诉渲染引擎工作时如何优化显示文本。 浏览器会在渲染速度、易读性（清晰度）和几何精度方面做一个权衡。

**语法**

```css
text-rendering: optimizeLegibility | auto | optimizeSpeed | geometricPrecision;
```

- `auto`：浏览器依照某些根据去推测在绘制文本时，何时该优化速度，易读性或者几何精度。对于该值在不同浏览器中解释的差异，请看兼容性表。
- `optimizeSpeed`：浏览器在绘制文本时将着重考虑渲染速度，而不是易读性和几何精度。它会使字间距和连字无效。Gecko 默认开启该属性，Firefox 是默认 20px 以下开启该属性。
- `optimizeLegibility`：浏览器在绘制文本时将着重考虑易读性，而不是渲染速度和几何精度。它会使字间距和连字有效。**该属性值在移动设备上会造成比较明显的性能问题**
- `geometricPrecision`：浏览器在绘制文本时将着重考虑几何精度， 而不是渲染速度和易读性。字体的某些方面—比如字间距—不再线性缩放，所以该值可以使使用某些字体的文本看起来不错。

```css
body {
  text-rendering: optimizeLegibility;
}
```

## 类似原生的 IOS 滚动

- `-webkit-overflow-scrolling: touch` 当手指从触摸屏上移开，会保持一段时间的滚动
- `-webkit-overflow-scrolling: auto` 当手指从触摸屏上移开，滚动会立即停止

这些属性只能在 Safari iOS 中使用

```css
body {
  -webkit-overflow-scrolling: touch;
  overflow-y: auto;
}
```

- [Scroll Bouncing On Your Websites](https://www.smashingmagazine.com/2018/08/scroll-bouncing-websites/)
- [Momentum Scrolling on iOS Overflow Elements](https://css-tricks.com/snippets/css/momentum-scrolling-on-ios-overflow-elements/)

## 蚀刻文字

创建一种效果，使文本看上去被“蚀刻”或雕刻到背景中。

- 使用 `text-shadow` 创建一个白色的影子偏移 `0px` 水平和 `2px` 从原始位置垂直。
- 为了使效果起作用，背景必须比阴影更暗。
- 文字颜色应略微褪色，使其看起来像是从背景上雕刻/雕刻出来的。

```css
.etched-text {
  text-shadow: 0 2px white;
  font-size: 1.5rem;
  font-weight: bold;
  color: #b8bec5;
}
```

## 实现文本溢出省略效果

- 单行文本溢出

```css
.text-ellipsis {
  width: 250px;
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
}
```

- 多行文本溢出

```css
.text-ellipsis {
  width: 250px;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
  text-overflow: ellipsis;
}
```

- 定位 + 渐变

```css
.truncate-text-multiline {
  position: relative;
  overflow: hidden;
  display: block;
  height: 109.2px;
  margin: 0 auto;
  font-size: 26px;
  line-height: 1.4;
  width: 400px;
  background: #f5f6f9;
  color: #333;
}

.truncate-text-multiline:after {
  content: '';
  position: absolute;
  bottom: 0;
  right: 0;
  width: 150px;
  height: 36.4px;
  background: linear-gradient(to right, rgba(0, 0, 0, 0), #f5f6f9 50%);
}
```

![溢出省略](https://upload-images.jianshu.io/upload_images/18281896-0d99d4c3a992f4c0.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 渐变文字

- `background` 与 `linear-gradient` 值一起使用可为文本元素提供渐变背景。
- `webkit-text-fill-color: transparent` 用于透明颜色填充文本。
- `webkit-background-clip: text` 用于用文本剪切背景，用渐变背景作为颜色填充文本。

```css
.gradient-text {
  background: linear-gradient(120deg, #fccb90 0%, #d57eeb 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}
```

![渐变文字](https://upload-images.jianshu.io/upload_images/18281896-a218fe9a96cddceb.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 文本描边效果

- `-webkit-text-stroke` 为文本字符指定了文本宽 `width` 和文本颜色 `color`
- `text-stroke-width`：设置或检索对象中的文字的描边厚度
- `text-stroke-color`：设置或检索对象中的文字的描边颜色

```css
.text {
  -webkit-text-stroke: 3px black;
}
```

效果如下：

![文本描边效果](https://upload-images.jianshu.io/upload_images/18281896-a41354ed2b62674a.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

[介绍文本笔画](https://www.webkit.org/blog/85/introducing-text-stroke/)

## 系统字体堆栈

使用本机的操作系统字体来接近本机应用程序的感觉。

- 使用 `font-family` 定义字体列表。
- 浏览器会查找每个连续的字体，如果可能的话，会选择第一个字体，如果找不到字体（在系统上或 CSS 中定义），则会退回到下一个字体。
- `-apple-system` 是 San Francisco,，在 iOS 和 macOS（而不是 Chrome）上使用。
- `BlinkMacSystemFont` 是 San Francisco，在 macOS Chrome 上使用。
- `'Segoe UI'` 在 Windows 10 上使用。
- `Roboto` 在 Android 上使用。
- `Oxygen-Sans` 在带有 KDE 的 Linux 上使用。
- `Ubuntu` 用于 Ubuntu （所有变体）。
- `Cantarell` 在带有 GNOME Shell 的 Linux 上使用。
- `'Helvetica Neue'` 并 `Helvetica`在 macOS 10.10 及更低版本上使用。
- `Arial` 是所有操作系统广泛支持的字体。
- 如果不支持其他字体，`sans-serif` 是后备无衬线字体。

```css
.system-font-stack {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto,
    Oxygen-Sans, Ubuntu, Cantarell, 'Helvetica Neue', Helvetica, Arial,
    sans-serif;
}
```

## 叠纸效果

```css
.paper {
  background: #fff;
  box-shadow: 0 1px 1px rgba(0, 0, 0, 0.15), 0 10px 0 -5px #eee,
    0 10px 1px -4px rgba(0, 0, 0, 0.15), 0 20px 0 -10px #eee,
    0 20px 1px -9px rgba(0, 0, 0, 0.15);
  padding: 30px;
}
```

效果如下：

![叠纸效果](https://upload-images.jianshu.io/upload_images/18281896-d830c066a31b3e68.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 隐藏数字输入微调器

- `::-webkit-inner-spin-button` CSS 伪元素用于设置数字选择器 `<input>` 元素的微调器按钮内部的样式。
- `::-webkit outer spin button` 设置外部样式
- `-webkit-appearance`：改变按钮和其他控件的外观，使其类似于原生控件。
- `::-webkit-inner-spin-button`、`::-webkit outer spin button` 和 `-webkit-appearance` 都是不规范的属性，它们没有出现在 CSS 规范草案中。有些浏览器不支持或渲染效果不同。

这里我们使用 [`-webkit-appearance`](https://developer.mozilla.org/en-US/docs/Web/CSS/appearance)，语法如下：

```css
-webkit-appearance：none | button | button-bevel ....
```

从视觉上隐藏数字选择器 `<input>` 元素的微调器：

```css
input[type='number']::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}
```

正常情况下：

![数字选择器](https://upload-images.jianshu.io/upload_images/18281896-4bc12089376f68f3.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

隐藏后：

![数字选择器](https://upload-images.jianshu.io/upload_images/18281896-25e89463b9905bd8.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 发光的蓝色输入亮点

```css
input[type='text'],
textarea {
  padding: 3px 0px 3px 3px;
  margin: 5px 1px 3px 0px;
  border: 1px solid #dddddd;
  outline: none;
  transition: all 0.3s ease-in-out;
}

input[type='text']:focus,
textarea:focus {
  padding: 3px 0px 3px 3px;
  margin: 5px 1px 3px 0px;
  border: 1px solid rgba(81, 203, 238, 1);
  box-shadow: 0 0 5px rgba(81, 203, 238, 1);
}
```

![发光的输入框](https://upload-images.jianshu.io/upload_images/18281896-d1e390d36742ffe8.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 非表单 fieldset 外观

```html
<section class="fieldset">
  <h1>This is not a fieldset</h1>
  <p>Booyah!</p>
</section>
```

CSS 如下：

```css
.fieldset {
  position: relative;
  border: 1px solid #ddd;
  padding: 10px;
}
.fieldset h1 {
  position: absolute;
  top: 0;
  font-size: 18px;
  line-height: 1;
  margin: -9px 0 0; /* half of font-size */
  background: #fff;
  padding: 0 3px;
}
```

效果如下：

![fieldset ](https://upload-images.jianshu.io/upload_images/18281896-b6f7ecd73b6b096d.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 将 WebKit 的文件上传输入按钮强制向右移动

```css
input[type='file'] {
  -webkit-appearance: none;
  -webkit-rtl-ordering: left;
  padding: 10px;
  border: 1px solid #3e68ff;
  border-radius: 8px;
  text-align: left;
}

input[type='file']::-webkit-file-upload-button {
  -webkit-appearance: none;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  padding: 0.25em 0.75em;
  margin: 0;
  float: right;
  border: none;
  background-color: transparent;
  cursor: pointer;
  min-width: 10ch;
  min-height: 44px;
  line-height: 1.1;
  background-color: #3e68ff;
  color: #fff;
  border-radius: 8px;
  box-shadow: 0 3px 5px rgba(0, 0, 0, 0.18);
}
```

![上传文件按钮右移](https://upload-images.jianshu.io/upload_images/18281896-3e7d94d8687c114f.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 简单而漂亮的 Blockquote 样式

```css
blockquote {
  background: #f9f9f9;
  border-left: 10px solid #ccc;
  margin: 1.5em 10px;
  padding: 1em 10px;
  quotes: '\201C''\201D''\2018''\2019';
}

blockquote:before {
  color: #ccc;
  content: open-quote;
  font-size: 4em;
  line-height: 0.1em;
  margin-right: 0.25em;
  vertical-align: -0.4em;
}

blockquote p {
  display: inline;
}
```

效果如下：

![简单而漂亮的 Blockquote 样式](https://upload-images.jianshu.io/upload_images/18281896-848e22c66dfb1ea0.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 使用纯 CSS 创建三角形

```css
.triangle {
  width: 0;
  height: 0;
  border-top: 20px solid #9c27b0;
  border-left: 20px solid transparent;
  border-right: 20px solid transparent;
}
```

效果如下：

![三角形](https://upload-images.jianshu.io/upload_images/18281896-1346666588eb3aca.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 制作一个列宽相等的表格

显式设置每个单元格的宽度是使所有列具有相同宽度的简单方法。例如，下面的 CSS 声明将包含四列的表拆分为宽度相同的部分：

```css
table td {
  width: 25%;
}
```

但是，如果表的列数是动态的，则该方法不起作用。幸运的是，我们可以使用 `table-layout` 属性来实现这一点。不管表有多少列，它们的宽度都是相同的。

```css
table {
  table-layout: fixed;
}
```

`table-layout: fixed` 可以让每个格子保持等宽：

```css
.calendar {
  table-layout: fixed;
}
```

无痛的 `table` 布局。

## 重置所有样式

仅使用一个属性将所有样式重置为默认值。

- 使用该`all`属性将所有样式（继承或不继承）重置为其默认值。
- **注意：**这不会影响 `direction`和`unicode-bidi`属性。

```css
.reset-all-styles {
  all: initial;
}
```
