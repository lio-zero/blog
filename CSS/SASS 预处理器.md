# SASS 预处理器

## 什么是 CSS 预处理器？

> CSS 预处理器定义了一种新的语言，其基本思想是，用一种专门的编程语言，为 CSS 增加了一些编程的特性，将 CSS 作为目标生成文件，然后开发者就只要使用这种语言进行编码工作。

- 通过工具编译成 CSS：开发后，这些特定文件被编译成任何浏览器都可以理解的常规 CSS。
- 能提升 CSS 文件的组织方式：预处理程序有助于在 CSS 中编写可重用，易于维护和可扩展的代码。
- 基于 CSS 的另一种语言：CSS 预处理器定义了一种新的语言，其基本思想是，用一种专门的编程语言，为 CSS 增加了一些编程的特性，将 CSS 作为目标生成文件，然后开发者就只要使用这种语言进行编码工作
- 扩展 CSS：CSS 预处理器为 CSS 增加一些编程的特性，无需考虑浏览器的兼容性问题，例如你可以在 CSS 中使用变量、简单的逻辑程序、函数、插值、mixin 等等在编程语言中的一些基本特性，可以让你的 CSS 更加简洁、适应性更强、可读性更佳，更易于代码的维护等。

目前主流的预处理器里有：[SASS（SCSS）](https://sass-lang.com/)、[Less](http://lesscss.org/)、[Stylus](https://stylus.bootcss.com/)、[PostCSS（后预处理器）](https://postcss.org/)

下面我们将来介绍 `SASS` 预处理器。详细内容查阅 [SASS 文档](https://sass-lang.com/documentation) 。

## 什么是 SASS？

> SASS 是最早的 CSS 预处理语言，有比 Less 更为强大的功能。但因其一开始的缩进式语法并不能被开发者们接受，所以使用率不高，不过由于其强大的功能和 Ruby on Rails 的大力推动，逐渐被更多开发者使用。

> SASS 是采用的 Ruby 语言编写的一款 CSS 预处理语言，它诞生于 2007 年，是最早成熟 CSS 预处理脚本语言，它被解释或编译成级联样式表。它允许您编写可维护的 CSS，并提供变量、嵌套、混合、扩展、函数、循环、条件等功能。最初它是为了配合 HAML 而设计的，因此有着和 HAML 一样的缩进式风格。

## SASS 和 SCSS 有什么区别？

> `SASS` 从第三代开始，放弃了缩进式风格，并且完全向下兼容普通的 `CSS` 代码，这一代的 `SASS` 也被称为 `SCSS`。`SCSS` 完全继承了 `SASS` 中的功能，并引入了一些新语法特性。

主要有两点不同：

- 文件扩展：SASS 的后缀扩展名为 `.sass`，而 SCSS 的 后缀为扩展名为 `.scss`
- 语法书写：SASS 是以严格的缩进式语法规则来书写，不带大括号 `{}` 和分号 `;`，而 SCSS 书写方式更像 CSS。

## 变量

`SASS` 变量很简单：您可以为以 `$` 开头的名称分配一个值，然后可以引用该名称而不是值本身。

```scss
$red: #833;

body {
  color: $red;
}
```

变量可以减少重复，进行复杂的数学运算，配置库等。

### CSS 变量与 SASS 变量的区别

- `SASS` 变量都是由 `SASS` 编译出来的。CSS 变量包含在 CSS 中输出。
- CSS 变量对于不同的元素可以有不同的值，但是 `SASS` 变量一次只有一个值。
- `SASS` 变量是必需的，这意味着如果您使用一个变量然后更改它的值，那么前面的使用将保持不变。CSS 变量是声明性的，这意味着如果您更改该值，它将影响前面使用和后面使用的。

## 嵌套

`SASS` 允许开发人员以嵌套的方式使用 CSS。

```scss
.markdown-body {
  a {
    color: blue;
    &:hover {
      color: red;
    }
  }
}
```

### 引用父级选择器 `&`

`SASS` 允许在嵌套的代码块内，使用 `&` 引用父元素

```scss
a {
  font-size: 14px;
  font-weight: bold;
  text-decoration: none;
  &:hover {
    text-decoration: underline;
    font-size: 20px;
  }
}
```

### 嵌套属性

CSS 许多属性都位于相同的命名空间，例如 `text-align`、`text-decoration`、`text-transform` 都位于 font 命名空间下，`SASS` 当中只需要编写命名空间一次，后续嵌套的子属性都将会位于该命名空间之下。

```scss
text: {
  align: center; // 将编译为 text-align: center
  decoration: underline; // 将编译为 text-decoration: underline
  transform: uppercase; // 将编译为 text-transform: uppercase
}
```

注意，`text` 后面必须加上冒号。

## 注释

```scss
// 单行注释，该注释方式只保留在 SASS 源文件中，编译后被省略。此注释不会包含在 CSS 中。

/* 多行注释，该注释方式会保留到编译后的文件。但压缩模式下，仍会被删除 */
/* 多行注释可以使用插值表达式 #{1 + 1} */

/*! 即使在压缩模式下也会包含此注释。*/
```

## 继承

`@extend` 指令用于继承另一个选择器的样式。相同样式直接继承，不会造成代码冗余；

```scss
.button {
  font-size: 14px;
  color: plum;
}

.push-button {
  @extend .button;
}
```

## 混合

混合（`Mixin`）允许您定义可以在整个样式表中重用的样式。

**语法**

```scss
@mixin  { ... }

// 带参数
@mixin name(<arguments...>) { ... }
```

**示例**

```scss
@mixin heading-font {
  font-family: sans-serif;
  font-weight: bold;
}

h1 {
  @include heading-font;
}
```

`Mixin` 特性在添加浏览器兼容性前缀的时候非常有用。

```scss
@mixin border-radius($radius) {
  border-radius: $radius;
  -ms-border-radius: $radius;
  -moz-border-radius: $radius;
  -webkit-border-radius: $radius;
}

.card {
  @include border-radius(8px);
}
```

### 参数

你可以向 `Mixin` 传递变量参数来让代码更加灵活。

```scss
@mixin font-size($n) {
  font-size: $n * 1.2em;
}

body {
  @include font-size(2);
}
```

### 具有默认值

```scss
@mixin pad($n: 10px) {
  padding: $n;
}

body {
  @include pad(15px);
}
```

### 具有默认变量

```scss
// 设置默认值
$default-padding: 10px;
@mixin pad($n: $default-padding) {
  padding: $n;
}

body {
  @include pad(15px);
}
```

## 引入 SCSS 模块

`SASS` 能够将代码分割为多个片段，并以下划线作为其命名前缀。`SASS` 会通过这些下划线来辨别哪些文件是 `SASS` 片段，并且不让片段内容直接生成为 CSS 文件，只在使用 `@import` 指令的位置被导入。

```scss
// _variables.scss
$gray-1: #eee;

// index.scss
@import './_variables';
```

`.scss` 或 `.sass` 扩展名是可选的。

## 函数

### 颜色功能

**颜色设置**

SASS 提供了一些内置的颜色函数，以便生成系列颜色。

```scss
// RGBA
rgb(100, 120, 140)
rgba(100, 120, 140, .5)
rgba($color, .5)

// 修改 HSLA
complement($color)    // 像 adjust-hue(_, 180deg)
invert($color)
grayscale($color)
hsla(hue, saturation, lightness, alpha)
```

**颜色操作**

```scss
// 混合
mix($a, $b, 10%)   // 10% a, 90% b

// 修改 HSLA
darken($color, 5%)
lighten($color, 5%)
saturate($color, 5%)
desaturate($color, 5%)
adjust-hue($color, 15deg)
fade-in($color, .5)   // 又名 opacify()
fade-out($color, .5)  // aka transparentize() - 将不透明度减半
rgba($color, .5)      // 将 alpha 设置为 .5

// 调整
// 按固定数量变动
adjust-color($color, $blue: 5)
adjust-color($color, $lightness: -30%)   // 像 darken(_, 30%)
adjust-color($color, $alpha: -0.4)       // 像 fade-out(_, .4)
adjust-color($color, $hue: 30deg)        // 像 adjust-hue(_, 15deg)

// 通过百分比更改
scale-color($color, $lightness: 50%)

// 完全更改一个属性
change-color($color, $hue: 180deg)
change-color($color, $blue: 250)
```

支持：`$red`、`$green`、`$blue`、`$hue`、`$saturation`、`$lightness`、`$alpha`

**颜色获取**

```scss
// HSLA
hue($color)         // 0deg-360deg 返回颜色在 HSL 色值中的角度值 (0deg - 255deg)。
saturation($color)  // 0%-100%
lightness($color)   // 0%-100%
alpha($color)       // 0-1 (类似 opacity())

// RGB
red($color)   // 从一个颜色中获取其中红色值（0-255）
green($color)
blue($color)
```

### 其他功能

**字符串**

```scss
unquote('hello')
quote(hello)
to-upper-case(hello)
to-lower-case(hello)
str-length(hello world)
str-slice(hello, 2, 5)      // "ello" - it's 1-based, not 0-based
str-insert("abcd", "X", 1)  // "Xabcd"
```

**单位**

```scss
unit(3em)        // 'em'
unitless(100px)  // false
```

**数字**

```scss
floor(3.5)
ceil(3.5)
round(3.5)
abs(3.5)
min(1, 2, 3)
max(1, 2, 3)
percentage(.5)   // 50%
random(3)        // 0..3
```

**选择器**

```scss
selector-append('.menu', 'li', 'a')   // .menu li a
selector-nest('.menu', '&:hover li')  // .menu:hover li
selector-extend(...)
selector-parse(...)
selector-replace(...)
selector-unify(...)
```

**其他**

```scss
variable-exists(red)    // 判断变量是否在当前的作用域下。
mixin-exists(red-text)  // 检测指定混入 (mixinname) 是否存在。
function-exists(redify) // 检测指定的函数是否存在
global-variable-exists(red) // 检测某个全局变量是否定义。
```

## 循环语句

SASS 提供了以下循环：

**for 循环**

```scss
@for $i from 1 through 4 {
  .item-#{$i} {
    left: 20px * $i;
  }
}
```

**each 循环（简单）**

```scss
$menu-items: home about services contact;

@each $item in $menu-items {
  .photo-#{$item} {
    background: url('images/#{$item}.jpg');
  }
}
```

**each 循环（嵌套）**

```scss
$backgrounds: (home, 'home.jpg'), (about, 'about.jpg');

@each $id, $image in $backgrounds {
  .photo-#{$id} {
    background: url($image);
  }
}
```

**while 循环**

```scss
$i: 6;
@while $i > 0 {
  .item-#{$i} {
    width: 2em * $i;
  }
  $i: $i - 2;
}
```

## 条件语句

```scss
@if $position == 'left' {
  position: absolute;
  left: 0;
} @else if $position == 'right' {
  position: absolute;
  right: 0;
} @else {
  position: static;
}
```

## 插值语句

通过 `#{}` 插值语句（interpolation）以在选择器或属性名中使用变量：

```scss
$name: card;
$attr: border;

.#{$class} {
  #{$attr}-color: plum;
}

@media #{$tablet}

font: #{$size}/#{$line-height}
url("#{$background}.jpg");
```

## 列表

> SASS 列表（List）函数用于处理列表，可以访问列表中的值，向列表添加元素，合并列表等。
>
> SASS 列表是不可变的，因此在处理列表时，返回的是一个新的列表，而不是在原有的列表上进行修改。
>
> 列表的起始索引值为 1，记住不是 0。

```scss
$list: (a b c);

nth($list, 1)  // 从 1 开始
length($list)  // 返回列表的长度

// 其他属性
nth // 函数可以直接访问数组中的某一项
join // 函数可以将多个数组连接在一起
list-separator(list) // 返回一列表的分隔符类型。可以是空格或逗号。
append(list, value, [separator]) // 将单个值 value 添加到列表尾部。separator 是分隔符，默认会自动侦测，或者指定为逗号或空格
is-bracketed(list) // 判断列表中是否有中括号
index(list, value) // 返回元素 value 在列表中的索引位置。
zip($list)  	// 将多个列表按照以相同索引值为一组，重新组成一个新的多维度列表。
set-nth(list, n, value) // 设置列表第 n 项的值为 value。

// 遍历列表中的每一项
@each $item in $list { ... }
```

## Map

> SASS Map（映射）对象是以一对或多对的 `key/value` 来表示。
>
> SASS Map 是不可变的，因此在处理 Map 对象时，返回的是一个新的 Map 对象，而不是在原有的 Map 对象上进行修改。

```scss
$map: (key1: value1, key2: value2, key3: value3);

map-get($map, key1)       // 返回 Map 中 key 所对应的 value(值)。如没有对应的 key，则返回 null 值。
map-has-key($map, key1)	  // 判断 map 是否有对应的 key，存在返回 true，否则返回 false。
map-keys($map)			  // 返回 map 中所有的 key 组成的队列。
map-merge($map1, $map2)	  // 合并两个 map 形成一个新的 map 类型，即将 map2 添加到 map1的尾部
map-remove($map, keys...) // 移除 map 中的 keys，多个 key 使用逗号隔开。
map-values($map)		  // 返回 map 中所有的 value 并生成一个队列。
```

## 更多资料

- [CSS Preprocessors Explained](https://www.freecodecamp.org/news/css-preprocessors/#:~:text=CSS%20Preprocessors%20compile%20the%20code,preprocessor%20were%20not%20in%20place.)
- [What is a CSS Preprocessors & Why Use Them](https://sherocommerce.com/what-is-a-css-preprocessors-why-use-them/)
