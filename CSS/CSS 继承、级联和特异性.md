# CSS 继承、级联和特异性

如果你想更好的掌握 CSS，CSS 的特异性是一个重要的话题。应用于 CSS 选择器的一组规则决定了应用于元素的样式。为了更好地理解这一点，我们必须了解一个相关的主题 —— CSS 中的[**级联和继承**](https://developer.mozilla.org/en-US/docs/Learn/CSS/Building_blocks/Cascade_and_inheritance)。

## CSS 的级联特性

CSS **层叠/级联样式表（cascade Stylesheets）**—— 简单的说，CSS 规则的顺序很重要；当应用两条同级别的规则到一个元素的时候，写在后面的就是实际使用的规则。我们来看一个例子，理解这一点。
我们将在一个元素中应用两个类，并为每个类提供一个不同的 `color`。

```html
<p class="color-pink color-plum">I'm Superman</p>
```

同时应用如下两个 CSS 类：

```css
.color-pink {
  color: pink;
}

.color-plum {
  color: plum;
}
```

效果如下：

![color-plum](https://upload-images.jianshu.io/upload_images/18281896-066db08968c00314.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**注意**，样式表中最后的 `color-plum` 将应用于元素。现在，您可能希望 CSS 总是这样将样式应用于元素，但情况并非总是如此。

```html
<p class="color-plum" id="paragraph">I'm Superman</p>
```

CSS 如下：

```css
#paragraph {
  color: pink;
}

.color-plum {
  color: plum;
}
```

您希望将哪种样式应用于元素？`#paragraph` 还是 `.color-plum`？

效果如下：

![paragraph](https://upload-images.jianshu.io/upload_images/18281896-74f8aec7533425d0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**注意**，应用了第一个。`#paragraph` 是一个 ID 选择器，而 `.color-plum` 是一个类选择器。这是因为级联具有特异性，来确定哪些值应用于元素。那么，CSS 的特异性是什么呢？

## CSS 继承的特性

[继承](https://developer.mozilla.org/en-US/docs/Web/CSS/inheritance)定义了哪些属性由它们所应用于的元素的子元素继承。
继承也需要在上下文中去理解 —— 一些设置在父元素上的 CSS 属性是可以被子元素继承的，有些则不能。

```html
<p>
  <span>I'm Superman</span>
</p>
```

CSS 如下：

```css
p {
  color: plum;
  font-size: 2rem;
  border: medium solid;
}
```

效果如下：

![继承](https://upload-images.jianshu.io/upload_images/18281896-4111dc7006a56929.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

可以看到，`<span>` 继承了 `color`，但是没继承 `border`，因为 `border` 就是一个典型的非继承属性，

这里顺带讲一下一些可以控制继承的一些属性。

CSS 为控制继承提供了四个特殊的通用属性值。每个 CSS 属性都接收这些值。

- `inherit` 设置该属性会使子元素属性和父元素相同。实际上，就是 "开启继承".
- `initial` 设置属性值和浏览器默认样式相同。如果浏览器默认样式中未设置且该属性是自然继承的，那么会设置为 `inherit` 。
- `unset` 将属性重置为自然值，也就是如果属性是自然继承那么就是 `inherit`，否则和 `initial`一样
- `revert` 只有很少的浏览器支持。

```html
<div>
  <h2>I'm Superman</h2>
</div>

<div class="extra">
  <h2>I'm Superman</h2>
</div>
```

```css
h2 {
  color: plum;
}

.extra h2 {
  color: inherit;
}
```

![inherit](https://upload-images.jianshu.io/upload_images/18281896-120ddf5b10264ca3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

可以看到，我们将 `<h2>` 的字体颜色设置为 `color: plum`，但是第二个 `<h2>` 确没有应用该颜色。原因是，我们在第二个 h2 中的 `color` 属性值使用了 `inherit`，该属性将继承父元素的值，没有设置将应用默认值。

## CSS 哪些属性可以继承？哪些属性不可以继承？

- 可以继承的样式：`font-size`、`font-family`、`color`、`list-style`、`cursor` 等。
- 不可继承的样式：`width`、`height`、`border`、`padding`、`margin`、`background` 等。

## CSS 特异性解释

> 根据 MDN，浏览器通过[**特异性（优先级）**](http://www.w3.org/TR/selectors/#specificity)来判断哪些属性值与一个元素最为相关，从而在该元素上应用这些属性值。特异性是基于不同种类选择器组成的匹配规则。

简单地说，如果两个 CSS 选择器应用于同一个元素，则使用具有更高特定性的一个。这就是为什么在我们前面的示例中，应用 ID 选择器的属性值，因为它具有更高的特异性值。

那么，如何计算选择器的特异性呢？

## 特异性层次结构

计算选择器的特异性值是相当棘手的。计算它的一种方法是使用权重系统对不同的选择器进行排序，以创建层次结构。我们将为每个选择器分配权重，以便更好地了解每个选择器的排名。让我们从最少的开始。

### 元素和伪元素

我们使用元素选择器（如 `a`、`p` 和 `div`）来设置所选元素的样式，而伪元素（如 `::after` 和 `::before`）用于设置元素特定部分的样式。

```css
/* 元素选择器 */
p {
  color: red;
}

/* 伪元素选择器 */
p::before {
  color: red;
}
```

元素和伪元素选择器的特异性最低。在特异性权重系统中，它的值为 1。

### 类、属性和伪类

以下是类、属性和伪类示例

```css
/* 类选择器 */
.person {
  color: plum;
}

/* 属性选择器 */
[type='radio'] {
  color: plum;
}

/* 伪类选择器 */
:focus {
  color: plum;
}
```

它比元素和伪元素选择器具有更高的特异性。在特异性权重系统中，它的值为 10。

### ID 选择器

ID 选择器用于使用元素的 ID 来定位元素。

```css
/* ID 选择器 */
#header {
  color: plum;
}
```

ID 选择器比类和元素具有更高的特异性。在特异性权重系统中，它的值为 100。

### 内联样式

内联样式直接应用于 HTML 文档中的元素。这是一个例子：

```html
<p style="color: plum">I'm Superman</p>
```

内联样式具有最高的特异性。在特异性权重系统中，它的值为 1000。

**总结**

```text
内联样式       - 1000
ID 选择器      - 100
类、属性和伪类  - 10
元素和伪元素    - 1
```

权重较高的选择器的属性值将始终应用于权重较低的选择器。内联样式具有最高的权重，其属性值将覆盖应用于元素的其他选择器的值。例如，如果我们有一个元素，对于相同的属性 `color`，就有一个内联样式。如果类和 ID 选择器也具有相同属性的值，则内联样式将获胜。

```html
<p style="color: purple" class="color-plum" id="paragraph">I'm Superman</p>
```

```css
#paragraph {
  color: pink;
}

.color-plum {
  color: plum;
}
```

效果如下：

![purple](https://upload-images.jianshu.io/upload_images/18281896-a2e73cb245fe730c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

当 ID 选择器和类选择器具有相同属性的值时，也会发生同样的情况。将应用 ID 选择器的属性值。

**注意**，仅当不同的选择器具有**相同属性**的值时，权重才适用。

### 多个元素选择器

有时，多个选择器用于定位元素。例如，下面的列表：

```html
<ul class="list">
  <li>Item 1</li>
  <li>Item 2</li>
  <li>Item 3</li>
</ul>
```

您可以针对这样的列表项目：

```css
.list > li {
  color: pink;
}
```

或

```css
ul > li {
  color: plum;
}
```

如果两个选择器都在同一个样式表上使用，将对列表项应用哪种样式？

让我们回到我们的权重系统来计算两个选择器的特异性。

对于`.list > li`，一个类选择器的权重是 10，一个元素选择器的权重是 1。它们的总和是 11。

对于 `ul > li`，一个元素选择器的权重为 1。使用两个元素选择器，因此它们的和为 2。

你认为哪些颜色值会被应用？

如果您说将应用 `.list > li` 选择器的颜色，那就对了。它比其他选择器具有更高的特异性值。

效果如下：

![pink](https://upload-images.jianshu.io/upload_images/18281896-dddbbed3898002c8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

让我们再试一个例子。鉴于此元素：

```html
<div class="first-block" id="box-1">
  <div class="second-block" id="box-2">
    <p class="text" id="paragraph">Item 1</p>
  </div>
</div>
```

CSS 如下：

```css
#box-1 > .second-block > .text {
  color: pink;
}

.first-block > #box-2 > #paragraph {
  color: plum;
}
```

试着计算特异性并猜测哪个 `color` 值适用。

效果如下：

![plum](https://upload-images.jianshu.io/upload_images/18281896-dd59d0ba5fba24b6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

让我们使用我们的权重系统来理解为什么应用第二个选择器的颜色值。

对于 `#box-1 > .second block > .text`，我们有一个 ID 选择器和两个类选择器。它们的权重之和是 `100 + 10 + 10 = 120`。

对于 `.first block > #div-2> #paragraph`，我们有一个类选择器和两个 ID 选择器。它们的权重之和是 `10 + 100 + 100 = 210`。

这就是为什么使用后一个选择器的值。

我们再来看一个例子，以确保你已经掌握。

```html
<div class="first-block" id="box">
  <ul class="first-list" id="list">
    <li class="first-list-item" id="list-item">
      First
      <span class="first-span" id="span">item</span>
    </li>
  </ul>
</div>
```

如果样式表中有以下样式，那么将对 `span` 应用哪种颜色？

```css
div#box > .first-list > #list-item > span {
  color: pink;
}

#list > #list-item > #span {
  color: purple;
}

#box > #list > .first-list-item > .first-span {
  color: plum;
}
```

尝试计算特异性，并将其与运行代码时得到的结果进行比较。

## 关于 CSS 特异性的重要点

- 分配给选择器的权重只是让我们知道将哪些规则应用到元素上。然而，这并不总是足够的。例如，您可以假设，如果使用 10 个以上的类（权重 >= 100）作为一个元素的目标，那么属性值将覆盖一个 ID 选择器的属性值。但事实并非如此。只要包含 10 个以上类的选择器且没有 ID 选择器，那么一个 ID 选择器将始终优先于它。
- `!important` — 任何选择器的属性值都使其成为将应用于元素的值。无论选择器在特异性层次结构上的排名如何，都会发生这种情况。让我们用一个例子来理解这一点。

```html
<p class="plum" id="paragraph" style="color: blue">Item 1</p>
```

如果应用以下样式

```css
p {
  color: pink !important;
}

.plum {
  color: plum;
}

#paragraph {
  color: purple;
}
```

元素选择器 `p` 的值将被使用，因为 `!important` 附加在价值上。然而，如果另一个选择器具有 `!important` 标记附加到相同的属性，则使用后面选择器的值。

![important](https://upload-images.jianshu.io/upload_images/18281896-d8581d40472552ce.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这就是为什么应该避免使用 `!important`，因为这样很难覆盖样式。

通常，要设置特定元素的样式，最好使用类。这使得在需要时更容易覆盖样式。

## 总结

1. 由于 CSS 的级联特性，如果对同一个元素应用两个规则，则最后一个规则就是将要使用的规则。
2. CSS 特异性是一组规则，用于确定哪个样式适用于元素。

3. 权重系统是计算不同选择器特异性的一种方法。

**以下是权重总结**

- 内联样式，如 `style="xxx"`，权值为 1000；
- ID 选择器，如 `#content`，权值为 100；
- 类、伪类和属性选择器，如 `.content`、`:hover`、`[attribute]`，权值为 10；
- 元素选择器和伪元素选择器，如`div`、`p`，权值为 1。

在同一组属性设置中，`!important` 优先级最高。无论选择器在何处，使用的特异性如何，将覆盖所有其他样式（包括行内样式）。

- 相同权重，定义最近者为准：行内样式 > 内部样式 > 外部样式 > 导入样式。
- 含外部载入样式时，后载入样式覆盖其前面的载入的样式和内部样式。
- 选择器优先级：`!important > id > class > tag`。

- **需要注意的是：通用选择器（`*`）、子选择器（`>`）和相邻同胞选择器（`+`）并不在权重系统中，所以他们的权值都为 0**。 权重值大的选择器其优先级也高，相同权重的优先级又遵循后定义覆盖前面定义的情况。
