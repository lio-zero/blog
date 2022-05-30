# 常用的一些 CSS 技巧二 — 选择器（伪类与伪元素）

你可以看看其他常用的 CSS 技巧：

- [常用的一些 CSS 技巧一](https://github.com/lio-zero/blog/blob/master/CSS/%E5%B8%B8%E7%94%A8%E7%9A%84%E4%B8%80%E4%BA%9B%20CSS%20%E6%8A%80%E5%B7%A7%E4%B8%80.md)
- [常用的一些 CSS 技巧三](https://github.com/lio-zero/blog/blob/master/CSS/%E5%B8%B8%E7%94%A8%E7%9A%84%E4%B8%80%E4%BA%9B%20CSS%20%E6%8A%80%E5%B7%A7%E4%B8%89.md)

## CSS 重置盒模型

```css
*,
*::before,
*::after {
  box-sizing: border-box;
}
```

当与将所有元素边距减为零的重置规则或父规则一起使用时，这会将上边距应用于其他元素后面的所有元素。这是一种快速获得垂直节奏的方法。

```css
* + * {
  margin-top: 1.5rem;
}
```

如果你真的想更具选择性，那么我喜欢在以下特定情况下将其作为后代使用：

```css
article * + h2 {
  margin-top: 4rem;
}
```

这类似于堆栈的思想，但更针对标题元素，以便在内容节之间提供更多的喘息空间。

## 清除浮动

- 使用 `:after` 伪元素并应用 `content: ''` 以使其影响布局。
- 使用 `clear: both` 做出明确的元素都过去左右浮动。
- 将该元素设置为 `display: block` 块级元素才能正常运行

```css
.clearfix::after {
  content: '';
  display: block;
  clear: both;
}
```

> **注意：**仅在用于 `float` 构建布局时，此选项才有用。考虑使用更现代的方法，例如 `flexbox` 或 `grid` 布局

## :focus 状态样式

```css
.input {
  transition: 180ms box-shadow ease-in-out;
}
.input:focus {
  box-shadow: 0 0 0 3px hsla(245, 100%, calc(82%), 0.8);
  border-color: hsl(245, 100%, 42%);
  outline: 3px solid transparent;
}
```

![:focus](https://upload-images.jianshu.io/upload_images/18281896-3857ab24f2a8c899.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## :focus-within

创建带有视觉，不可编辑前缀的输入。

- 当用户与 `<input>` 字段交互时，使用 `:focus-within` 伪类选择器为父元素设置样式。

```html
<div class="input-box">
  <span class="prefix">+08</span>
  <input type="tel" placeholder="888 8888" />
</div>
```

css 如下

```css
.input-box {
  display: flex;
  align-items: center;
  max-width: 300px;
  background: #fff;
  border: 1px solid #a0a0a0;
  border-radius: 4px;
  padding-left: 0.5rem;
  overflow: hidden;
  font-family: sans-serif;
}

.input-box .prefix {
  font-weight: 300;
  font-size: 14px;
  color: #999;
}

.input-box input {
  flex-grow: 1;
  font-size: 14px;
  background: #fff;
  border: none;
  outline: none;
  padding: 0.5rem;
}

.input-box:focus-within {
  border-color: plum;
}
```

![:focus-within](https://upload-images.jianshu.io/upload_images/18281896-e56eebdb2db5ca70.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 实现分割线

实现分割线的方式有很多种，这里我们使用 `::before` 和 `::after` 伪元素实现

```css
.divider {
  display: flex;
  align-items: center;
}

.divider::before,
.divider::after {
  content: '';
  flex: 1;
  height: 1px;
  background: #dcdfe6;
}

.divider::before {
  margin-right: 1rem;
}

.divider::after {
  margin-left: 1rem;
}
```

![分割线](https://upload-images.jianshu.io/upload_images/18281896-64451b14844cfa41.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

更多方法：[CSS 巧妙实现分隔线的几种方法](https://www.daqianduan.com/4258.html) 和 [CSS 巧妙实现自适应分隔线的 N 种方法](https://www.jb51.net/css/706905.html)

## CSS `:empty`

通常，我们希望为包含内容的元素设置样式。当元素完全没有子元素或文本时，该怎么办？您可以使用 `:empty` 选择器：

```html
<p></p>
<!-- 注意空白区 -->

<p></p>
<!-- 中间什么都没有 -->
```

```css
p::before {
  font-family: 'FontAwesome';
  content: '\f240';
}

p:empty::before {
  content: '\f244';
}

p {
  color: silver;
}

p:empty {
  color: red;
}
```

### 显示空列表的占位符

通过使用 `:empty` 选择器，我们可以显示自定义占位符。

```css
ul:empty::after {
  content: 'Empty';
}
```

如果希望占位符更灵活，而不是在 CSS 中硬编码，则可以从属性中获取占位符，例如 `data-placeholder`：

```
ul:empty::after {
  content: attr(data-placeholder);
}
```

如果列表包含空格或空子节点，则 `:empty` 选择器无效。

### 设置空链接的内容

对于内容为空的链接，我们可以使用 `href` 属性替换内容：

```css
a[href^='http']:empty:before {
  content: attr(href);
}
```

## CSS :only-child

如果要设置没有兄弟元素的样式，请使用 `:only-child` 伪类选择器

```html
<ul>
  <li>child</li>
</ul>

<ul>
  <li>siblings</li>
  <li>siblings</li>
</ul>
```

```css
li:only-child {
  color: DeepPink;
}
```

## CSS :not()

不要使用两个不同的选择器来指定样式，然后使用另一个来否定使用它。`:not` 选择器可选择除与所传递参数匹配的元素之外的所有元素

```css
/* ❌ */
li {
  margin-left: 10px;
}

li:first-of-type {
  margin-left: 0;
}

/* ✅ 使用 :not() 要好很多 */
li:not(:first-of-type) {
  margin-left: 10px;
}
```

## 逗号分隔列表

使列表的每项都由逗号分隔：

```css
ul > li:not(:last-child)::after {
  content: ',';
}
```

因最后一项不加逗号，可以使用 `:not()` 伪类。

> **注意**： 这一技巧对于无障碍，特别是屏幕阅读器而言并不理想。而且复制粘贴并不会带走 CSS 生成的内容，需要注意。

## 移除最后一个导航项的边框

我们经常使用 `:last-child` 选择器取消应用最后一项的特定样式（例如 `border`）。创建每个项目都有底部边框的导航通常如下所示：

```css
li {
  border-bottom: 1px solid #e5e7eb;
}

/* 不向最后一项添加边框 */
li:last-child {
  border-bottom: none;
}
```

使用 `:not` 伪类，我们可以通过一个 CSS 声明使代码更短、更易于维护：

```css
/* 将边框添加到除最后一个项目外的所有项目 */
li:not(:last-child) {
  border-bottom: 1px solid #e5e7eb;
}
```

另一种方法是使用 `+` 选择器：

```css
li + li {
  border-top: 1px solid #e5e7eb;
}
```

## 隐藏没有静音、自动播放的影片

这是一个自定义用户样式表的不错的技巧。避免在加载页面时自动播放。如果没有静音，则不显示视频：

```css
video[autoplay]:not([muted]) {
  display: none;
}
```

## 单独的项目列表

`:after` 选择器的内容属性可用于分隔列表项：

```css
/* 内联项 */
span:not(:last-child):after {
  content: ' • ';
}

/* items 列表 */
li:not(:last-child):after {
  content: ',';
}
```

## CSS 设置 ::placeholder 文本的样式

使用 `::placeholder` 伪元素在 `<input>` 或 `<textarea>` 元素为占位符文本设置样式 。大多数现代浏览器都支持此功能，但对于较旧的浏览器，将需要供应商前缀。

```html
<input placeholder="CSS Placeholder" />
```

```css
::placeholder {
  color: pink;
}
```

![placeholder ](https://upload-images.jianshu.io/upload_images/18281896-eb0a27ff2f59f166.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## CSS :placeholder-shown

使用 `:placeholder-shown` 伪类为当前显示占位符文本的输入设置样式

```css
input:placeholder-shown {
  border-color: pink;
}
```

在逛到张鑫旭的博客时，看到一篇关于 [`:placeholder-shown`](https://www.zhangxinxu.com/wordpress/2018/12/css-placeholder-shown-material-design/) 的文章，里面示例的代码主要如下：

```css
/* 默认placeholder颜色透明不可见 */
.input-fill:placeholder-shown::placeholder {
  color: transparent;
}

.input-fill-x {
  position: relative;
}
.input-label {
  position: absolute;
  left: 16px;
  top: 14px;
  pointer-events: none;
}

.input-fill:not(:placeholder-shown) ~ .input-label,
.input-fill:focus ~ .input-label {
  transform: scale(0.75) translate(0, -32px);
}
```

![:placeholder-shown](https://upload-images.jianshu.io/upload_images/18281896-7705f8d2e9e5bce5.gif?imageMogr2/auto-orient/strip)

[`:placeholder-shown` 实现占位符过渡动画 demo](https://www.zhangxinxu.com/study/201812/placeholder-shown-label-transition-demo.php)

## 自定义列表

主要的几行代码如下：

```css
ul li::marker {
  content: attr(data-icon);
  font-size: 1.25em;
}

ol li::marker {
  content: counter(list-item);
  font-family: 'Indie Flower';
  font-size: 1.5em;
  color: purple;
}
```

上面例子中，`attr()` 用于抓取标签上的自定义属性 `data-*` 中的值赋予 content。详细例子请看：[CODEPEN](https://codepen.io/5t3ph/pen/KKgqeNB)

![1616732729(1).jpg](https://upload-images.jianshu.io/upload_images/18281896-849c1096ef2a2dcd.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 自定义文字选择

使用 `::selection` 伪选择器来选择其中的文本样式。

```css
::selection {
  background: aquamarine;
  color: black;
}

.custom-text-selection::selection {
  background: deeppink;
  color: white;
}
```

对于 Firefox，您将需要使用 `::-moz-selection`

```css
p::-moz-selection {
  background: deeppink;
  color: white;
}
```

## 自定义 WebKit 滚动条

为具有可滚动溢出的元素自定义滚动条样式。

**主要有三个**

- `::-webkit-scrollbar` 设置整个滚动条元素的样式。
- `::-webkit-scrollbar-track` 滚动条轨道（滚动条的背景）的样式。
- `::-webkit-scrollbar-thumb` 滚动条滑块（可拖动元素）的样式。

**其他**

- `::-webkit-scrollbar-button` 设置滚动条上的按钮 (上下箭头)的样式。
- `::-webkit-scrollbar-track-piece` 设置滚动条没有滑块的轨道部分的样式。
- `::-webkit-scrollbar-corner` 设置当同时有垂直滚动条和水平滚动条时交汇的部分的样式。
- `::-webkit-resizer` 设置某些元素的 corner 部分的部分样式（如：textarea 的可拖动按钮）的样式。

```css
/* 为所以的滚动条设置相同的样式 */
::-webkit-scrollbar {
  width: 8px;
  background: transparent;
}

::-webkit-scrollbar-track {
  background: #e0e4f6;
  border-radius: 12px;
}

::-webkit-scrollbar-thumb {
  background: #666e8f;
  border-radius: 12px;
}

/* 仅在单独地方设置滚动条样式 */

.box::-webkit-scrollbar {
  width: 8px;
  background: transparent;
}
.box::-webkit-scrollbar-track {
  background: #e0e4f6;
  border-radius: 12px;
}
.box::-webkit-scrollbar-thumb {
  background: #666e8f;
  border-radius: 12px;
}
```

**更多资料：**

- [Scrollbar Shenanigans](https://www.swyx.io/scrollbar-shenanigans/)
- [Strut Your Stuff With a Custom Scrollbar](https://css-tricks.com/strut-your-stuff-with-a-custom-scrollbar/)
- [scrollbar](https://css-tricks.com/almanac/properties/s/scrollbar/)

## 首字大写

- `::first-line` 伪元素选择器选择换行之前文本的首行。
- `::first-letter` 伪元素选择器应用于元素中文本的首字母。
- `:first-child` 伪类选择器仅选择第一段

```css
p::first-line {
  color: plum;
}
```

![first-line](https://upload-images.jianshu.io/upload_images/18281896-569c428b8b475b14.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```css
p::first-letter {
  color: plum;
  font-size: 40px;
}
```

![first-letter](https://upload-images.jianshu.io/upload_images/18281896-f615e7c1fbd1c934.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```csc
p:first-child {
  color: plum;
}
```

![first-child](https://upload-images.jianshu.io/upload_images/18281896-1ca352523054117d.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

使第一段的第一个字母大于文本的其余部分。

- 使用 `:first-child` 选择器仅选择第一段。
- 使用 `::first-letter` 伪元素为段落的第一个元素设置样式。

```css
p:first-child::first-letter {
  color: plum;
  float: left;
  margin: 0 8px 0 4px;
  font-size: 3rem;
  font-weight: bold;
  line-height: 1;
}
```

![首字大写](https://upload-images.jianshu.io/upload_images/18281896-715c53c472d5c380.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 只显示第一个字母

有些情况下，我们希望在生成的 HTML 中填充全文，但只显示第一个字母。其余的字符将在视觉上隐藏。例如，当我们必须支持更小的屏幕时，它非常有用。

这里的技巧是为元素设置零字体大小，而第一个字母的字体大小是正常的。

```css
.element {
  font-size: 0;
}

.element::first-letter {
  font-size: 1.5rem;
}
```

## 使用 `:nth-child()` 创建具有交替背景颜色的列表

使用 `:nth-child(odd)` 或 `:nth-child(even)` 伪类选择器将不同 `background-color` 的元素应用于根据其在同级组中的位置匹配的元素。

```css
li:nth-child(odd) {
  background-color: purple;
}
li:nth-child(even) {
  background: plum;
}
```

![1616744697(1).jpg](https://upload-images.jianshu.io/upload_images/18281896-a79b0cb4b8a7fcb8.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 使用负 `:nth-child` 和 `:nth-last-child` 来选择元素

使用负的  `nth-child`  可以选择 1 至 n 个元素。

在以下示例中，前三项将添加下划线。第二到第五项的范围为蓝色。

```css
li:nth-child(-n + 3) {
  text-decoration: underline;
}

li:nth-child(n + 2):nth-child(-n + 5) {
  color: #2563eb;
}
```

我们先隐藏所有的 `li` 标签，然后选择第 1 至第 3 个元素并显示出来

```css
li {
  display: none;
}

li:nth-child(-n + 3) {
  display: block;
}
```

选择除前 3 个之外的所有项目，并隐藏它们

```css
li:not(:nth-child(-n + 3)) {
  display: none;
}
```

同样地，`nth-last-child` 会选择最后几个子项。

```css
/* 在最后两项中添加 decorative */
li:nth-last-child(-n + 2) {
  text-decoration-line: line-through;
}
```

> [查看效果](https://codepen.io/lio-zero/pen/oNeqYzP)

## :hover

- 使用 `transition` 设置不透明度更改的动画。
- 使用 `:hover` 和 `:not` 伪类选择器将所有元素的不透明度更改为 0.5，但鼠标停留的元素除外。

```css
li {
  display: inline-block;
  padding: 0 16px;
  transition: opacity 0.3s;
}
.sibling-fade:hover li:not(:hover) {
  opacity: 0.5;
}
```

![sibling-fade](https://upload-images.jianshu.io/upload_images/18281896-1419b95aa0d1825b.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

`:hover` 可以配合动画和其他选择器做出很多效果，找时间在写一些例子，顺便加深印象

## ::marker

`::marker` 伪元素代表一个列表项的标记框 `<li>`。标记通常是一个项目符号或数字。

```css
ul li::marker {
  color: red;
  font-size: 1.5em;
}
```

![::marker](https://upload-images.jianshu.io/upload_images/18281896-fec616a19c4707a0.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 使用 :invalid 和 :valid 校验表单元素

- CSS 伪类 `:invalid` 表示任意内容未通过验证的 `<input>` 或其他 `<form>` 元素。
- CSS 伪类 `:valid` 表示内容验证正确的 `<input>` 或其他 `<form>` 元素。

```html
<form>
  <div>
    <label for="url_input">Enter a URL: </label>
    <input type="url" id="url_input" />
  </div>
</form>
```

```css
/* 当校验错误时 */
input:invalid {
  background-color: pink;
}
form:invalid {
  border: 2px solid pink;
  padding: 10px;
}

/* 当校验成功时 */
input:valid {
  background-color: #ddffdd;
}
form:valid {
  border: 2px solid #ddffdd;
  padding: 10px;
}
```

当校验错误时：

![1616748752(1).jpg](https://upload-images.jianshu.io/upload_images/18281896-208b947e9179e423.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

当校验成功时：

![1616748801(1).jpg](https://upload-images.jianshu.io/upload_images/18281896-021466bf1b81684c.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 使用 :checked 和 `:default` 赋予元素状态

`:checked` 伪类选择器表示任何处于选中状态的 `radio`，`checkbox` 或 `select` 元素中的 `option`。

```css
option:checked {
  color: coral;
}
```

![:checked](https://upload-images.jianshu.io/upload_images/18281896-dc60359daf4f3163.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

`:default` 伪类选择器只能作用在表单元素上，表示默认状态的表单元素。

`:default` 默认应用到有 checked 属性的的标签上。当你切换或选择其他选项时，该选择器不会跟随着应用到你选择的标签上。（它是固定的，不响应的）

**示例：默认推荐**

```css
input:default + label:after {
  content: ' (推荐)';
  color: coral;
}
```

![:default](https://upload-images.jianshu.io/upload_images/18281896-549649f4d87cafd1.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 将前导零附加到有序列表项

将 `list-style-type` 属性设置为以下值将为有序列表 ( `ol`) 的项追加零编号：

```css
ol {
  list-style-type: decimal-leading-zero;
}
```

然而，它只对指数小于 10 的项目有效。这意味着，如果我们的列表中有 100 多个项目，那么它们的前缀如下：

```
01. Item 02. Item ... 09. Item 10. Item ... 99. Item 100. Item ...
```

为了解决这个问题，我们可以使用 CSS 计数器。每项保存计数器的当前值，该值在下一项中递增 1：

```css
ol {
  counter-reset: items;
  list-style-type: none;
}

li {
  counter-increment: items;
}
```

要使用关联计数器值作为项的前缀，请使用 `::before` 伪元素。

```css
li:before {
  content: '00' counter(items) '. ';
}

li:nth-child(n + 10)::before {
  content: '0' counter(items) '. ';
}

li:nth-child(n + 100)::before {
  content: counter(items) '. ';
}
```

`:nth-child(n+10)` 选择器指示索引大于或等于 10 的项。它将覆盖应用于 `li::before` 元素的样式。以同样的方式，`:nth-child(n+100)` 覆盖了 `:nth-child(n+10)` 的样式。

## 使用特殊字符设置列表项的样式

我们通常使用圆形或方形来设置列表项的样式，如下所示：

```css
li {
  list-style-type: circle;
}
```

您知道列表样式类型属性也接受字符吗。这意味着我们可以使用表情符号或 Unicode 字符：

```css
li {
  list-style-type: '☀️';
}

/* 或 */
li {
  list-style-type: '\2600';
}
```

## 使用 “形似猫头鹰” 选择器

这个名字可能比较陌生，不过通用选择器（`*`）和相邻兄弟选择器（`+`）一起使用，效果非凡：

```css
* + * {
  margin-top: 1.5em;
}
```

在此示例中，文档流中的所有的相邻兄弟元素都将设置 `margin-top: 1.5em` 的样式。

更多 “形似猫头鹰” 的选择器，可参考 _A List Apart_ 上面 [Heydon Pickering 的文章](http://alistapart.com/article/axiomatic-css-and-lobotomized-owls)

## 将样式与 :is 伪类选择器组合

`:is` 伪类选择器为与参数中列出的选择器匹配的任何元素应用样式。

而不是编写单独的选择器：

```css
header a:hover,
nav a:hover,
footer a:hover {
  text-decoration: underline;
}
```

我们可以将它们合并为一个，如下所示：

```css
:is(header, nav, footer) a:hover {
  text-decoration: underline;
}
```

## 使用 `:root` 选择器灵活控制字体大小

在响应式布局中，字体大小应需要根据不同的视口进行调整。你可以计算字体大小根据视口高度和宽度，这时需要用到 `:root`：

```css
:root {
  font-size: calc(1vw + 1vh + 0.5vmin);
}
```

现在，您可以使用 **root em**

```css
body {
  font: 1rem/1.6 sans-serif;
}
```

## 利用属性选择器来选择空链接

当 `<a>` 元素没有文本内容，但有 `href` 属性的时候，显示它的 `href` 属性：

```css
a[href^='http']:empty::before {
  content: attr(href);
}
```

## 利用 `+` 隐藏额外的换行符

利用相邻兄弟选择器（`+`），在用作间距的换行符（`br`）上设置 `display: none`，有助于防止 CMS 用户使用额外的换行符。

```css
br + br {
  display: none;
}
```

## 在行内元素之间添加换行符

这是一个常见的场景，我们想把一个标题分成多行。例如，标题在大屏幕上连续显示。但在小屏幕上，它应该分成不同的部分。

在不使用 `br` 标签的情况下，我们可以从各种内联 `span` 元素构建标题。

```html
<h2>
  <span class="primary">Eet, sleep, code</span>
  <span>Travel around the world</span>
</h2>
```

通过使用 `::after` 伪元素，我们可以在第一个行内元素之后添加换行符：

```css
.primary::after {
  content: '\A';
  white-space: pre;
}
```

其中 `\A` 表示换行符。
