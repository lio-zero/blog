# BEM 命名规范

众所周知，CSS 在大型、复杂、快速迭代的系统中难以管理。有多种编写 CSS 的方法可以编写更易于维护的 CSS。

本文将来了解 CSS BEM 命名规范。

BEM 是一种前端项目开发的方法论，由 [Yandex](http://yandex.ru/) 公司提出。

BEM 的名称来源于该方法学的三个组成部分的英文首字母，分别是块（Block）、元素（Element）和修饰符（Modifier）。

这里先推荐一篇关于使用 BEM 的组件命名规范的示例文章：[bem naming cheat sheet by 9elements](https://9elements.com/bem-cheat-sheet/#form-blocks)。

![bem naming cheat sheet by 9elements](https://upload-images.jianshu.io/upload_images/18281896-17eb45f3195525d4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

其中介绍了包括：面包屑、按钮、卡片、列表、导航、布局、表单控件等一些组件的结构、命名示例。

## 什么是 CSS BEM？

**[BEM](https://bem.info/)**（Block Element Modifier，块元素修饰符）是 CSS 类的命名约定，旨在通过定义命名空间来解决范围问题来使 CSS 更具可维护性。

它原则上建议为独立的 CSS 类命名，并且在需要层级关系时，将关系也体现在命名中，这自然会使选择器高效且易于覆盖。

- block（块）是一个独立的组件，可在项目中重复使用，并充当子组件（元素）的 "命名空间"。
- 当 block（块）或 element（元素）处于特定状态或结构或样式不同时，将 modifier（修饰符）用作标志。

## BEM 命名约定

- `block` 代表了更高级别的抽象或组件。
- `block__element` 代表 `.block` 的后代，用于形成一个完整的 .block 的整体。
- `block--modifier` 代表 `.block` 的不同状态或不同版本。

```css
.block {}
.block__element {}
.block--modifier {}
```

## 示例

- BEM 实体名称全部是小写字母或数字。名称中的不同单词用单个连字符（`-`）分隔。
- BEM 元素名称和块名称之间通过两个下划线（`__`）分隔。
- 布尔修饰符和其所修饰的实体名称之间通过两个连字符（`--`）来分隔。不使用名值对修饰符。

```html
<ul class="menu">
  <li class="menu__item menu__item--selected">Item 1</li>
  <li class="menu__item">Item 2</li>
  <li class="menu__item">Item 3</li>
</ul>
```

CSS 如下：

```css
.menu {
  list-style: none;
}
.menu__item {
  font-weight: bold;
}
.menu__item--selected {
  color: plum;
}
```

分析：

- `.menu` 封装一个独立的实体，它本身是有意义的。虽然块可以嵌套并相互交互，但在语义上它们是相等的；没有优先级或等级制度。
- `.menu__item` 块的一部分，没有独立的意义。任何元素在语义上都与其块相关联。
- `.menu__item--selected` 块或元素上的修饰符。使用它们来改变外观、行为或状态。

## BEM 命名规范带来的好处

BEM 的优点在于所产生的 CSS 类名都只使用一个类别选择器，可以避免传统做法中由于多个类别选择器嵌套带来的复杂的属性级联问题。

换句话说，其所有样式规则的**特异性（specificity）**都是相同的，也就不存在复杂的优先级问题，简化了层叠规则。如果你还不是很了解特异性的话，可以查阅之前写的一篇 [CSS 继承、级联和特异性](https://github.com/lio-zero/blog/blob/master/CSS/CSS%20%E7%BB%A7%E6%89%BF%E3%80%81%E7%BA%A7%E8%81%94%E5%92%8C%E7%89%B9%E5%BC%82%E6%80%A7.md)。

细分后，可以从**模块化**、**可重用性**、**结构**三部分进行理解：

- **模块化**：块样式从不依赖于页面上的其他元素，因此您将永远不会遇到级联带来的问题。
- **可重用性**：以不同的方式构成独立的块，并以智能方式对其进行重用，从而减少了必须维护的 CSS 代码量。
- **结构**：BEM 方法为您的 CSS 代码提供了坚实的结构，使结构保持简单易懂。

## 其他的一些命名规范

除了 BEM 以外，还有其他一些常用命名规范如：[OOCSS](http://oocss.org/)、[SMACSS](https://smacss.com/) 等。

### OOCSS

OOCSS 表示的是面向对象 CSS（**Object Oriented CSS**），是一种把面向对象方法学应用到 CSS 代码组织和管理中的实践

**OOCSS 有两个重要的原则**：

- 第一个原则是把结构和外观分开。
- 第二个原则是把容器和内容分开。

### SMACSS

SMACSS 表示的是可扩展和模块化 CSS（**Scalable and Modular Architecture for CSS**）。

SMACSS 把 CSS 样式规则分成若干个不同的类别：

- **基础**：该类别中包含的是默认的 CSS 样式。作为其他样式的基础。
- **布局**：该类别中包含与页面布局相关的 CSS 样式，用来进行模块的排列。
- **模块**：该类别中包含的是可复用的模块的 CSS 样式。
- **状态**：该类别中的 CSS 样式用来描述布局和模块在不同状态下的外观。比如在不同的屏幕尺寸下，布局会发生变化。标签式模块的每个标签页可以有显示或隐藏的状态。
- **主题**：该类别和状态类似，只不过是用来改变布局和模块的视觉效果。

## CSS 架构

这一节总结一下目前前端的一些 CSS 架构：

- [BEM](http://getbem.com/)
- [CSS Modules](https://github.com/css-modules/css-modules)
- [Atomic](https://acss.io/)
- [OOCSS](https://github.com/stubbornella/oocss/wiki)
- [SMACSS](https://smacss.com/)
- [SUITCSS](https://suitcss.github.io/)

另外，可以了解一下近几年很火的 [CSS-in-JS](https://github.com/lio-zero/blog/blob/main/WTF/%E4%BB%80%E4%B9%88%E6%98%AF%20CSS-in-JS.md)。

## 更多资源

- [A Look at Some CSS Methodologies](https://www.webfx.com/blog/web-design/css-methodologies/)
- [如何看待 CSS 中 BEM 的命名方式？](https://www.zhihu.com/question/21935157/answer/20116700)
- [BEMIT: Taking the BEM Naming Convention a Step Further](https://csswizardry.com/2015/08/bemit-taking-the-bem-naming-convention-a-step-further/)
- [BEM Docs](https://en.bem.info/methodology/quick-start)
- [BEM 101](https://css-tricks.com/bem-101)
- [BEM TUTORIALS](https://en.bem.info/tutorials/)
- [Modern alternatives to BEM](https://daverupert.com/2022/08/modern-alternatives-to-bem/)
