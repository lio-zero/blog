# CSS 选择器

> [CSS 选择器](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/Building_blocks/Selectors)是 CSS 规则集的一部分，它实际上选择要设置样式的内容。

## CSS 选择符

**通用选择器**：`*` 将匹配文档的所有元素。不推荐使用通配选择器，因为它是性能最低的一个 CSS 选择器。

```css
/* 常用的是清除所有元素的 margin 和 padding */
* {
  margin: 0;
  padding: 0;
}

/* 也可以与子选择器一起使用。 */
.container * {
  border: 1px solid black;
}
```

**元素选择器**：选择具有给定节点名称的所有元素。

```css
/* input 将匹配任何 <input> 元素 */
input { }
```

**ID 选择器**：根据其 `id` 属性值选择一个元素。文档中应只有一个具有给定 ID 的元素。

```css
/* 将匹配 ID 为 app 的元素 */
#app { }
```

**类选择器**：选择具有给定 `class` 属性的所有匹配的元素。

```css
/* 将匹配具有 .active 类的任何元素 */
.active { }
```

**后代选择器**：` `（空格）选择器选择前一个元素的后代。

```css
/* 将匹配 div 元素内部的所有 span 元素（深层嵌套也将被匹配到） */
div span { }
```

**子代选择器**：`>` 选择器选择前一个元素的直接子代的节点。

```css
/* 匹配直接嵌套在 ul 元素内的所有 li 元素（深层嵌套将匹配不到） */
ul > li { }
```

**选择器列表**：`,` 是将不同的选择器组合在一起的方法，它选择所有能被列表中的任意一个选择器选中的节点。

```css
/* 会同时匹配 <div> 元素和 <span> 元素 */
div, span { }
```

**通用兄弟选择器**：`~` 选择器选择兄弟元素，位置无须紧邻，只须同层级。

```css
/* 匹配同一父元素下，p 元素后的所有 span 元素 */
p ~ span { }
```

**相邻兄弟选择器**：`+` 选择器选择相邻元素，当第二个元素*紧跟在*第一个元素之后，并且两个元素都是属于同一个父元素的子元素，则第二个元素将被选中。

```css
/* 会匹配所有相邻在 h2 元素后的 p 元素 */
h2 + p { }
```

**属性选择器**：按照给定的属性，选择所有匹配的元素。

```css
/* 选择所有具有 autoplay 属性的元素（不论这个属性的值是什么） */
[autoplay] { }
```

**伪类**：`:` 伪选择器是添加到选择器的关键字，指定要选择的元素的特殊状态。

```css
/* 匹配所有曾被访问过的 <a> 元素 */
a:visited { }
```

**伪元素**：`::` 伪元素选择器用于表示无法使用 HTML 语义表达的实体。

```css
/* 匹配所有 <p> 元素的第一行 */
p::first-line { }
```

## 组合选择器

> 组合选择器是将两个选择器连接在一起的选择器中的字符。组合选择器有四种类型。

- **后代选择器（空格）**
- **子代选择器（>）**
- **通用兄弟选择器（~）**
- **相邻兄弟选择器（+）**

用法上一节已经讲到。

## 属性选择器

CSS 属性选择器通过已经存在的属性名或属性值匹配元素。

```css
/* 匹配 title 属性的 <a> 元素 */
a[title] {
  color: pink;
}

/* 匹配 href 属性并且属性值为 http:.... 的 <a> 元素 */
a[href='https://github.com/lio-zero/blog'] {
  color: plum;
}

/* 匹配 lang 属性并且属性值为 zh-*（连字符）的 div 元素，例如：zh-CN 或 zh-TW */
div[lang|='zh'] {
  color: plum;
}

/* 将所有语言为美国英语的 <div> 元素的文本颜色设为蓝色 */
div[lang~="en-us"] {
  color: blue;
}

/* 匹配 href 属性并且属性值包含 www 的 <a> 元素 */
a[href*='www'] {
  color: pink;
}

/* 匹配 href 属性并且属性值开头是 https 的 <a> 元素 */
a[href^='https'] {
  color: pink;
}

/* 匹配 src 属性并且属性值结尾是 .jpg 的 <img> 元素 */
img[src$='.jpg'] {
  color: pink;
}
```

匹配方式类似正则。

属性选择器有多种匹配方式，这节进行扩展介绍。

## 伪选择器

### 伪类

```css
p:lang(language) 为 <p> 元素的 lang 属性选择一个开始值

input:focus 选择元素输入后具有焦点
a:link 选择所有未访问链接
a:visited 选择所有访问过的链接
a:active 选择正在活动链接
a:hover 把鼠标放在链接上的状态
input:valid  选择所有有效值的属性
#line:target 选择当前活动 #line 元素(点击 URL 包含锚的名字)
input:required 选择有 required 属性指定的元素属性
input:read-write 选择没有只读属性的元素属性
input:read-only 选择只读属性的元素属性
input:out-of-range 选择指定范围以外的值的元素属性
input:optional 选择没有 required 的元素属性

/* CSS3 新增选择器 */
input[type="text"] 属性选择器
:root 选择文档的根元素，等同于 html 元素
:empty 选择没有子元素的元素
:target 选取当前活动的目标元素
:not(selector) 选择除 selector 元素意外的元素

/* 表单 */
input:enabled 选择可用的表单元素
input:disabled 选择禁用的表单元素
input:checked 选择被选中的表单控件元素(单选框或复选框)

p:nth-child(n) 匹配父元素下指定子元素，在所有子元素中排序第 n。还有 add、oven
p:nth-last-child(n) 选择所有 p 元素倒数的第二个子元素
p:nth-of-type(n) 选择所有 p 元素第二个为 p 的子元素
p:nth-last-of-type(n) 选择所有 p 元素倒数的第二个为 p 的子元素

p:first-child 选择器匹配属于任意元素的第一个子元素的 p 元素
p:last-child 选择所有 p 元素的最后一个子元素
p:only-child 选择所有仅有一个子元素的 p 元素
p:only-of-type 选择属于其父元素唯一的 p 元素的每个 p 元素
p:first-of-type 选择的每个 p 元素是其父元素的第一个 p 元素
p:last-of-type 选择属于其父元素的最后 p 元素的每个 p 元素
```

### 伪元素

```css
p::selection 选择用户选择的元素部分。
p::before 在每个 p 元素之前插入内容
p::after 在每个 p 元素之后插入内容
p::first-line 选择每个 p 元素的第一行
p::first-letter 选择每个 p 元素的第一个字母
```

## 常见的问题

### 伪类和伪元素之间有什么区别？

伪元素和伪类之所以这么容易混淆，是因为他们的效果类似而且写法相仿。

CSS3 为了区分两者，明确规定伪类用一个冒号（`:`）来表示，而伪元素则用两个冒号（`::`）来表示。

但因为兼容性的问题，所以现在大部分还是统一的单冒号，但是抛开兼容性的问题，我们在书写时应该尽可能养成好习惯，区分两者。

表现形式上作区分：伪类表现的是某种状态被选择，例如 `:hover`、`:checked`，而伪元素表现的是选择元素的某个部分，使这部分看起来像一个独立的元素，例如 `::before` 、`::after`。抽象的说，伪类就是选择元素某种状态，伪元素就是创建一个 HTML 元素。

还需要注意：

- IE 6-7 不支持 `:focus`
- 伪类名称对大小写不敏感
- 伪元素也有人称为伪对象

关于伪类/伪元素的示例可以查阅我最近新开的另一个仓库 [css-tricks](https://github.com/lio-zero/css-tricks)，里面提供了大量的 CSS 技巧（包括伪类/伪元素），也可以阅读 [An Ultimate Guide To CSS Pseudo Classes And Pseudo Elements](https://www.smashingmagazine.com/2016/05/an-ultimate-guide-to-css-pseudo-classes-and-pseudo-elements/) 文章。

### CSS `content` 属性有什么作用？

`content` 属性专门应用在 `before/after` 伪元素上，用于插入额外内容或样式。

```css
/* <div data-icon="icon">...</div> */
a::before {
  content: attr(data-icon);
}
```

### CSS 选择器的优先级（特异性）是什么？

> MDN：浏览器通过**优先级**来判断哪些属性值与一个元素最为相关，从而在该元素上应用这些属性值。优先级是基于不同种类选择器组成的匹配规则。

### CSS 权重和优先级

权重分为四级，分别是：

- 内联样式，如 `style="xxx"`，权值为 1000
- ID 选择器，如 `#content`，权值为 100
- 类、伪类和属性选择器，如 `.content`、`:hover` 和 `[attribute]`，权值为 10
- 元素选择器和伪元素选择器，如 `div`、`p`，权值为 1

相同权重，以定义最近者为准：行内样式 > 内部样式 > 外部样式 > 导入样式

- 含外部载入样式时，后载入样式将覆盖其之前载入的样式和内部样式（只覆盖相同的 CSS 选择器）
- 选择器优先级：`!important > id > class > tag`
- 在同一组属性设置中，`!important` 优先级最高，高于行内样式

> **注意：通用选择器（`*`）、子选择器（`>`）和相邻兄弟选择器（`+`）并不在这四个等级中，所以他们的权值都为 0**。权重值大的选择器其优先级也高，相同权重的优先级又遵循后定义覆盖前面定义的情况。
>
> 详细内容请查看：[优先级](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Specificity) 和 [CSS3 选择器优先级](http://www.w3.org/TR/selectors/#specificity)

### 浏览器如何解析 CSS 选择器？

浏览器**从右往左**解析 CSS 选择器，这样的匹配节点的方式能快速、准确的与 render 树上的节点进行匹配，避免了许多无效匹配。浏览器需要评估的规则越少，样式引擎执行的速度就越快。

> 详细内容可查看：[Why do browsers match CSS selectors from right to left?](https://stackoverflow.com/questions/5797014/why-do-browsers-match-css-selectors-from-right-to-left)

### 浏览器如何解析 CSS？

浏览器如何解析 CSS？
