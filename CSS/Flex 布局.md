# Flex 布局

Flex 是 Flexible Box 和分配容器中项目之间的空间，即使这些项目的大小未知或是动态。

Flex 布局背后的主要思想是让容器能够改变其项目的宽度/高度（和顺序），以最好地方式填充可用空间（主要是为了适应各种显示设备和屏幕大小）。flex 容器扩展项目以填充可用空间，或收缩项目以防止溢出。

最重要的是，Flex 布局与常规布局（基于垂直的块和基于水平的内联）相比是方向无关的。虽然这些方法对页面很有效，但它们缺乏灵活性来支持大型或复杂的应用程序（尤其是在方向改变、调整大小、拉伸、收缩等方面）。

Flex 布局在 2009 年由 W3C 提出的。它在所有的浏览器都支持：

![Flex 支持情况](https://upload-images.jianshu.io/upload_images/18281896-edeae6574fa01df3.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

> **注意**：Flexbox 布局最适合应用程序的组件和小规模布局，而 Grid 布局则适用于大规模布局。

以下内容参考 [Flexbox30](https://www.samanthaming.com/flexbox30/)，图片截自 [A Complete Guide to Flexbox](https://css-tricks.com/snippets/css/a-guide-to-flexbox/)

## 基础概念

为了使 Flexbox 正常工作，您需要设置父子关系。父级是 Flex 容器，其中的所有内容都是子级或 Flex 项。

**Flex 容器**仅环绕其直接子容器。因此，没有孙子或孙辈的关系。只有父母 <-> 直系子女！只要存在父子关系，就可以建立 Flexbox。因此，孩子也可以成为其孩子的伸缩容器。但这将是一个单独的 flex 容器。而且它不会继承祖父母的 flex 属性。

还有一点，关于为什么不能将文本容器设置为 flexbox 容器，可以阅读 [Never make your text container a flexbox container](https://dev.to/afif/never-make-your-text-container-a-flexbox-container-m9p)。

![flexbox 属性应用示意图](https://upload-images.jianshu.io/upload_images/18281896-d907e938cdf1d8b7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

Flexbox 在 2 轴系统中运行：水平的主轴（main axis）和垂直的交叉轴（cross axis）。主轴是您将伸缩项目如何放置在伸缩容器中的定义方向。确定横轴非常简单，它是在垂直于主轴的方向上进行的。

记住不要把他比作数学上的 **x** 和 **y** 轴。因为 **x** 轴并不总是主轴。这可能会让你出错。

在每个轴上都有一个起点和终点。如果在主轴上，则将起始位置称为 **main start**，将结束位置称为 **main end**。相同的概念适用于交叉轴。知道起点和终点很重要，因为您可以控制 flex 项目的放置位置。

项目默认沿主轴排列。单个项目占据的主轴空间叫做 `main size`，占据的交叉轴空间叫做 `cross size`。

## 属性

父容器和子项目都有自己的一套属性。

父容器：

- `flex-direction`
- `flex-wrap`
- `flex-flow`
- `justify-content`
- `align-items`
- `align-content`

子项目：

- `flex-grow`
- `flex-shrink`
- `flex-basis`
- `flex`
- `order`
- `align-self`

下面，我们会对它们一一讲解。

## 父容器

![父容器](https://upload-images.jianshu.io/upload_images/18281896-17095dabca7fddc9.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

flex 容器有 2 种类型：`flex` 将创建一个块级 flex 容器，`inline-flex` 将创建一个 inline 级 flex 容器。

```css
.parent {
  display: flex /* default */ | inline-flex;
}
```

很简单地解释，块元素占据了容器的整个宽度。它们看起来像构建块，其中每个构建块彼此堆叠。内联元素仅占用其所需的空间。因此，它们似乎排成一行，或者彼此并排。

### flex-direction

`flex-direction` 定义主轴的属性。记住主轴可以是水平或垂直的。因此，如果我们希望主轴是水平的，则称为行。如果我们希望它是垂直的，那就叫做列。另外，请记住我们有一个主要的起点和终点。我们只需添加一个反向后缀即可将 **main start** 设置为反向。

```css
.parent {
  flex-direction: row /* default */ | row-reverse | column | column-reverse;
}
```

![flex-direction](https://upload-images.jianshu.io/upload_images/18281896-dc080b31fbfebcbb.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### flex-wrap

默认情况下，弹性项目将尝试使其自身收缩以适合一行，换句话说，不进行换行。但是，如果您希望 flex 项保持其大小并在容器中的多行中溢出，则可以使用 `flex-wrap: warp;`。此属性将使容器中的弹性项目占用多行。

```css
.parent {
  flex-wrap: nowrap /* default */ | wrap | wrap-reverse;
}
```

![flex-wrap](https://upload-images.jianshu.io/upload_images/18281896-78de20fd7401631e.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

`flex-wrap` 允许 flex 项在单独的行上进行包装。但有了 `align-content` 属性，我们可以控制哪些项目行在横轴上的对齐方式。由于这仅适用于包装的项目，所以如果只有一行 flex 项，则此属性不会有任何效果。

### flex-flow

`flex-flow` 是 `flex-direction` 和 `flex-wrap` 的简写。默认值为 row nowrap。如果仅设置一个值，则未设置的属性将采用默认值。

```css
.parent {
  flex-flow: row nowrap /* default */ | <flex-direction> <flex-wrap> | <flex-direction> |
    <flex-wrap>;
}
```

### justify-content

`justify-content` 设置沿主轴对齐的属性。

```css
.parent {
  justify-content: flex-start /* default */ | flex-end | center | space-around | space-between |
    space-evenly;
}
```

![justify-content](https://upload-images.jianshu.io/upload_images/18281896-93eed65c0780566c.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

主轴也可以垂直放置。在这种情况下，将 `flex-direction` 设置为 `column`。

```css
.parent {
  flex-direction: column;
  justify-content: flex-start /* default */ | flex-end | center | space-around | space-between |
    space-evenly;
}
```

### align-items

`align-items` 设置沿横轴对齐的属性。记住横轴始终垂直于主轴。

```css
.parent {
  align-items: stretch /* default */ | flex-start | flex-end | center | baseline;
}
```

![align-items](https://upload-images.jianshu.io/upload_images/18281896-5576f5eecc138a73.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

现在，让我们看一下如果交叉轴水平放置，则弹性项目如何对齐。换句话说，伸缩方向是列。

```css
.parent {
  flex-direction: column;
  align-items: stretch /* default */ | flex-start | flex-end | center | baseline;
}
```

### align-content

`align-content` 属性定义了多根轴线的对齐方式。如果项目只有一根轴线，该属性不起作用。

```css
.parent {
  align-content: stretch /* default */ | flex-start | flex-end | center | space-between |
    space-around;
}
```

![align-content](https://upload-images.jianshu.io/upload_images/18281896-e69c1724d4bd3616.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### space-evenly 和 space-around

在 `space-evenly` 中，弹性项目之间的空白空间始终相等。

![space-evenly](https://upload-images.jianshu.io/upload_images/18281896-6a1282084a079582.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

但是，在 `space-around` 中，只有内部项目之间的间距相等。第一项和最后一项将仅分配一半的间距。使其具有更加分散的视觉外观：

![space-around](https://upload-images.jianshu.io/upload_images/18281896-710888f334b0e3e3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 子项目

![子项目](https://upload-images.jianshu.io/upload_images/18281896-032d8f99bc950f09.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### order

默认情况下，弹性项目的显示顺序与在代码中显示的顺序相同。但是，如果您要更改该怎么办？没问题！使用 `order` 属性更改项目的顺序。

```css
.child {
  order: 0 /* default */ | <number>;
}
```

![order](https://upload-images.jianshu.io/upload_images/18281896-4c9ebe0ac0136af5.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### flex-grow

Flexbox 非常适合响应式设计。`flex-grow` 属性允许我们的 flex 项在必要时增长。因此，如果我的容器中有多余的可用空间，我可以告诉某个特定项目按一定比例将其填满。

```css
.child {
  flex-grow: 0 /* default */ | <number>;
}
```

![flex-grow](https://upload-images.jianshu.io/upload_images/18281896-283032d1033b5b7f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### flex-shrink

如果有伸缩空间，`flex-grow` 将会扩展以填充额外的空间。相反 `flex-shrink` 当空间不足时将会控制 flex 项目缩小到适合的程度。请注意，数字越大，缩小幅度越大。

```css
.child {
  flex-shrink: 1 /* default */ | <number>;
}
```

你可以在 [flex-grow-calculation](https://www.samanthaming.com/flexbox30/22-flex-grow-calculation/) 和 [flex-shrink-calculation](https://www.samanthaming.com/flexbox30/24-flex-shrink-calculation/) 查看浏览器如何帮我们自动处理 `flex-grow`、`flex-shrink`。

### flex-basis

使用 `flex-basis` 属性可以设置项目的初始大小。您可以将此属性视为 flex 项目的宽度。

因此，您的下一个问题可能是 `width` 和 `flex-basis` 之间的区别是什么。当然，您仍然可以使用 `width`，它将仍然有效。

它起作用的原因是，如果未设置 `flex-basis`，它将默认为 `width`。因此，您的浏览器将始终尝试查找 `flex-basis` 值作为大小指示符。如果找不到它，那么它别无选择，只能使用 `width` 属性。不要让浏览器做额外的工作。使用适当的 flex 方法并使用 `flex-basis`。

```css
.child {
  flex-basis: auto /* default */ | <width>;
}
```

当一个项目具有 `flex-basis` 和 `width` 时，浏览器将始终使用 `flex-basis` 设置的值。但要注意，如果同时设置了 `min-width` 和 `max-width`。在这些情况下，`flex-basis` 将丢失，并且不会用作宽度。

### flex

`flex` 属性是上面所提到的 `flex-grow`、`flex-shrink` 和 `flex-basis` 的简写形式。如果你足够了解它们的特性，请使用简写吧！

```css
.child {
  flex: 1 0 auto /* default */ | <flex-grow> <flex-shrink> <flex-basis> | <flex-grow> | <flex-basis>
    | <flex-grow> <flex-basis> | <flex-grow> <flex-shrink>;

  /* 相当于： */
  flex-grow: 1;
  flex-shrink: 0;
  flex-basis: auto;
}
```

### align-self

`align-items` 属性可以沿着横轴设置 flex 项。`align-items` 的问题是它强制所有 flex 项使用规则。

但是如果你想让他们中的一个打破规则，你可以使用 `align-self`。此属性接受为 `align-items` 提供的所有相同值，因此您可以轻松脱离包装 😎

```css
.child-1 {
  align-self: stretch /* default */ | flex-start | flex-end | center | baseline;
}
```

![align-self](https://upload-images.jianshu.io/upload_images/18281896-cce21923c0d44473.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## flex 示例

### 垂直水平居中元素

方式一：

```css
.container {
  display: flex;
  justify-content: center; /* horizontal */
  align-items: center; /* vertical */
}
```

方式二：

```css
.container {
  display: flex;
}

.container > div {
  margin: auto;
}
```

![水平垂直居中](https://upload-images.jianshu.io/upload_images/18281896-12c537f47dfc6abf.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

对齐 Flexbox 子元素的另一种方法是使用自动外边距。尽管这不是 Flexbox 属性，但要意识到这一点仍然很重要，因为它与 Flexbox 有非常有趣的关系。[Bonus: Aligning with Auto Margins](https://www.samanthaming.com/flexbox30/31-flexbox-with-auto-margins/)

### 重新排序

```css
.container > .top {
  order: 1;
}

.container > .bottom {
  order: 2;
}
```

### 截断文本

```scss
.parent {
  display: flex;
  align-items: center;
  padding: 10px;
  margin: 30px 0;
}

.child {
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.child > h2 {
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.truncated {
  flex: 1;
  h2 {
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
  }
}
```

[查看效果](https://codepen.io/lio-zero/pen/OJWXYzG)

### 移动布局

创建固定高度的顶栏和动态高度的内容区域。

```css
.container {
  display: flex;
  flex-direction: column;
}

.container > .top {
  flex: 0 0 60px;
}

.container > .content {
  flex: 1 0 auto;
}
```

### 流式布局

自动用适当数量的盒子填充可用空间。

```css
.container {
  display: flex;
  flex-wrap: wrap;
  align-content: flex-start;
  gap: 1rem;
}

.container > * {
  flex: 1 1 10ch;
}
```

![流式布局](https://upload-images.jianshu.io/upload_images/18281896-4b2aadd6f1437555.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

[查看效果](https://codepen.io/lio-zero/pen/NWdKNyJ)

### 响应导航栏

```css
.nav {
  display: flex;
  flex-flow: row wrap;
  justify-content: flex-end;

  list-style: none;
  margin: 0;
  background: deepskyblue;
}

.nav a {
  text-decoration: none;
  display: block;
  padding: 1em;
  color: white;
}

.nav a:hover {
  background: #1565c0;
}

@media all and (max-width: 800px) {
  .nav {
    justify-content: space-around;
  }
}

@media all and (max-width: 600px) {
  .nav {
    flex-flow: column wrap;
    padding: 0;
  }
  .nav a {
    text-align: center;
    padding: 10px;
    border-top: 1px solid rgba(255, 255, 255, 0.3);
    border-bottom: 1px solid rgba(0, 0, 0, 0.1);
  }
  .nav li:last-of-type a {
    border-bottom: none;
  }
}
```

[演示地址](https://codepen.io/lio-zero/pen/xxWMNVX)

### 固定脚部

HTML 结构：

```html
<section>
  <header></header>
  <main></main>
  <footer></footer>
</section>
```

方式一：

```css
section {
  display: flex;
  flex-direction: column;
}

main {
  flex-grow: 1;
  /* flex: 1 0 auto; */
}

footer {
  flex-shrink: 0;
}
```

方式二：

```css
section {
  display: flex;
  flex-direction: column;
}

footer {
  margin-top: auto;
}
```

> 推荐：[固定页脚](https://github.com/lio-zero/blog/blob/main/CSS%20Layout/%E5%9B%BA%E5%AE%9A%E9%A1%B5%E8%84%9A.md)

### 砌体布局

```scss
.masonry-container {
  display: flex;
  flex-direction: column;
  flex-wrap: wrap;
  max-height: 600px;
  .masonry-brick {
    margin: 0 0.825rem 0.825rem 0;

    border-radius: 1rem;
  }
}

/* or */

.masonry-container {
  display: flex;
  flex-wrap: wrap;
  .masonry-brick {
    flex: 1 0 auto;
    height: 150px;
    margin: 0 0.825rem 0.825rem 0;
    border-radius: 1rem;
  }
}
```

![砌体布局](https://upload-images.jianshu.io/upload_images/18281896-81f3607683a8348f.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

[查看效果](https://codepen.io/lio-zero/pen/PobMdVE)

### 圣杯布局

HTML 结构：

```html
<div class="container">
  <header>Lorem</header>
  <main>
    <aside>Lorem</aside>
    <article>Lorem</article>
    <nav>Lorem</nav>
  </main>
  <footer>Lorem</footer>
</div>
```

CSS 样式：

```css
.container {
  display: flex;
  flex-direction: column;
}

main {
  display: flex;
  flex-direction: row;
  flex-grow: 1;
}

aside {
  width: 25%;
}

article {
  flex-grow: 1;
}

nav {
  width: 20%;
}
```

[演示地址](https://codepen.io/lio-zero/pen/JjLxqKM)

您也可以使用 `order` 去调整排列顺序。

### 双飞翼布局

双飞翼布局其实是根据圣杯布局演化出来的一种布局。

```css
.container {
  display: flex;
  flex-flow: row nowrap;
  justify-content: space-around;
  height: 100%;
}

.left,
.right {
  width: 200px;
  flex-shrink: 1;
}

.main {
  flex-grow: 1;
}
```

![双飞翼布局](https://upload-images.jianshu.io/upload_images/18281896-8859b7e288a661e5.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

[查看效果](https://codepen.io/lio-zero/pen/xxgKORY)

### 两列布局

```css
.container {
  display: flex;
}

.left {
  width: 200px;
  height: 100%;
}

.right {
  flex: 1;
  height: 100%;
}
```

![两列布局](https://upload-images.jianshu.io/upload_images/18281896-3d629d61426aec35.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

[查看效果](https://codepen.io/lio-zero/pen/yLVmWRY)

## 更多资料

- [Basic concepts of flexbox](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Flexible_Box_Layout/Basic_Concepts_of_Flexbox)
- [flex 备忘单](https://www.sketchingwithcss.com/samplechapter/cheatsheet.html)
- [Flex 布局教程：语法篇](https://www.ruanyifeng.com/blog/2015/07/flex-grammar.html)
- [Flex 布局教程：实例篇](https://www.ruanyifeng.com/blog/2015/07/flex-examples.html)
- [What Happens When You Create A Flexbox Flex Container?(opens new window)](https://www.smashingmagazine.com/2018/08/flexbox-display-flex-container/)
- [Everything You Need To Know About Alignment In Flexbox(opens new window)](https://www.smashingmagazine.com/2018/08/flexbox-alignment/)
- [Flexbox: How Big Is That Flexible Box?(opens new window)](https://www.smashingmagazine.com/2018/09/flexbox-sizing-flexible-box/)
- [Use Cases For Flexbox](https://www.smashingmagazine.com/2018/10/flexbox-use-cases/)
