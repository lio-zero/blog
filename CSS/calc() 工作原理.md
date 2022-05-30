# calc() 工作原理

CSS3 `calc()` 函数允许我们在声明 CSS 属性值时执行以下计算，也就数学运算（`+`、`-`、`*`、`/`）。

例如，我们不必声明元素宽度的静态像素值，我们可以使用 `calc()` 来指定宽度是两个或多个数值相加的结果。

```css
.foo {
  width: calc(100px + 20px);
}
```

## `calc()`

如果您使用过像 SASS 这样的 CSS 预处理器，你肯定遇到过上面的这种写法。

```scss
.nav {
  width: 100px + 20px;
}

// SASS 中可以直接相加
.foo {
  width: 100px + 20px;
}

// 或者使用变量
$first-width: 100px;
$second-width: 20px;

.bar {
  width: $first-width + $second-width;
}
```

但是，`calc()` 函数提供了一个更好的解决方案，原因有两个。

- **我们可以合并不同的单位**。具体来说，我们可以将相对单位（如百分比（`%`）和视口单位（`vw`、`vh`、`vmax`、`vmin`））与绝对单位（如像素（`px`））混合。例如，我们可以创建一个表达式，用百分比值减去像素值。

> 单位详细介绍可查看：[CSS 单位及其需要注意的地方](https://github.com/lio-zero/blog/blob/master/CSS/CSS%20%E5%8D%95%E4%BD%8D%E5%8F%8A%E5%85%B6%E9%9C%80%E8%A6%81%E6%B3%A8%E6%84%8F%E7%9A%84%E5%9C%B0%E6%96%B9.md)。

```css
.sidebar {
  width: calc(100% - 120px);
}
```

在本例中，`.sidebar` 元素的宽度总是比其父元素宽度的 `100%` 小 `120px`。

- 对于 `calc()`，**计算的值是表达式本身**，而不是表达式的结果值。使用 CSS 预处理器处理数学表达式时，给浏览器的值就是表达式的结果值。

```scss
// SCSS 中指定的值
.foo {
  width: 100px + 20px;
}

// 在浏览器中编译 CSS 和计算值
.foo {
  width: 120px;
}
```

但是，浏览器解析的值是实际表达式。

```scss
// CSS 中指定的值
.foo {
  width: calc(100% - 120px);
}

// 浏览器中的计算值
.foo {
  width: calc(100% - 120px);
}
```

这意味着浏览器中的值可以更动态，并随着视口的变化而调整。我们可以有一个高度为视口减去绝对值的元素，它将随着视口的变化而调整。

## 使用 `calc()`

`calc()` 函数可用于对数值执行**加减乘除**计算。也就是可以与 `<length>`、`<frequency>`、`<angle>`、`<time>`、`<number>`、`<integer>` 一起使用。

```css
.foo {
  width: calc(50vmax + 3rem);
  padding: calc(1vw + 1em);
  transform: rotate(calc(1turn + 28deg));
  background: hsl(100, calc(3 * 20%), 40%);
  font-size: calc(50vw / 3);
}
```

### 嵌套 `calc()`

`calc()` 函数可以嵌套。但是，内部函数将被视为简单的括号表达式。以下面的嵌套表达式为例

```css
.foo {
  width: calc(100% / calc(100px * 2));
}
```

此函数的计算值如下

```css
.foo {
  width: calc(100% / (100px * 2));
}
```

### 提供后备方案

对 `calc()` 的支持相对广泛，以下为其支持情况：

![calc 支持情况](https://upload-images.jianshu.io/upload_images/18281896-94c1f36b7872f4a2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

基本上现代主流浏览器均支持该属性。但对于那些不支持的浏览器（你知道的），将忽略整个属性值表达式。这也意味着我们可以很容易地提供一个将由不支持的浏览器使用的回退静态值。

```css
.foo {
  width: 90%; /* 旧浏览器的回退 */
  width: calc(100% - 120px);
}
```

## 应用

`calc()` 函数在各种情况下都很有用。

### 定位元素

使用 `calc()` 为解决容器内元素水平垂直居中提供了另一种解决方法。如果我们已知子元素的尺寸，一个典型的解决方案是使用负边距将元素的高度和宽度移动一半，就像这样

```scss
.foo {
  position: absolute;
  top: 50%;
  left: 50%;
  margin-top: -150px;
  margin-left: -150px;
  width: 300px;
  height: 300px;
}
```

使用 `calc()` 函数，我们只需在顶部和左侧使用属性即可实现所有这些操作。

```scss
.foo {
  position: absolute;
  top: calc(50% - 150px);
  left: calc(50% - 150px);
  width: 300px;
  height: 300px;
}
```

随着 Flexbox 的引入，这样的方法不太可能被需要。但是，在不能使用 Flexbox 的情况下，例如，如果元素需要绝对定位或固定，则此方法非常有用。

### 创建根网格大小

`calc()` 函数可用于在单元外创建基于视口的网格。我们可以将根元素的字体大小设置为整个视口宽度的一小部分。

```css
html {
  font-size: calc(100vw / 30);
}
```

现在，将关联到视口宽度的 `1/30`。对于我们页面上的任何文本，这意味着它将根据视口自动缩放。此外，给定相同的视区尺寸，无论视区的实际大小如何，屏幕上始终会有相同数量的文本。

如果我们使用 `rem` 单位来调整页面上其他非文本元素的大小，它们也会遵循这种行为。宽度为 `1rem` 的元素始终为视口宽度的 `1/30`。

### 清晰度

`calc()` 有助于使任何正在进行的计算更加明显。例如，如果希望一组项的宽度为其父容器宽度的 1/6，可以这样编写

```css
.foo {
  width: 16.666666667%;
}
```

然而，阅读 CSS 的人更清楚的是如何编写

```css
.foo {
  width: calc(100% / 6);
}
```

### 创建网格系统

对于创建 Grid 网格系统来说，`calc` 在好不过：

```css
img {
  float: left;
  margin: 10px;
  width: calc(100% * 1 / 4 - 20px);
}

@media (max-width: 900px) {
  img {
    width: calc(100% * 1 / 3 - 20px);
  }
}

@media (max-width: 600px) {
  img {
    width: calc(100% * 1 / 2 - 20px);
  }
}

@media (max-width: 400px) {
  img {
    width: calc(100% - 20px);
  }
}
```
