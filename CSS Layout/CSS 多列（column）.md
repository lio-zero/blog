# CSS 多列（column）

CSS `columns` 属性用来设置元素的列宽和列数。

## 浏览器支持情况

## 相关属性

- `column-width: <length unit>` 列的宽度。
- `column-count: <number> | auto` 指定元素的列数。
- `columns` 为 `column-width` 和 `column-count` 的简写形式
- `column-rule-style` 指定两列间边框的样式
- `column-rule-width` 指定两列间边框的厚度
- `column-rule-color` 指定两列间边框的颜色
- `column-rule` 为上面三个的简写形式，规定列之间的宽度、样式和颜色。
- `column-gap` 控制列之间的距离
- `column-span: none | all` 指定元素要跨越多少列
- `column-fill: all | balanced` 指定如何填充列

您可以使用 `break-before`，`break-after` 或者 `break-inside` 来如何控制每一列中的内容显示。

默认情况下，列与内容保持平衡。如果您希望列不平衡，可以使用 `column-fill: auto` 进行设置。其默认行为为 `column-fill: balanced`。

```css
.container {
  width: 300px;
  margin: 100px auto;
  columns: 8;
  column-rule: 1px dashed rgb(213, 213, 213);
  direction: rtl;
  word-wrap: break-word;
  text-align: center;
}
```

> [查看效果](https://codepen.io/lio-zero/pen/mdRpNjm)

我们再来看看两个例子。

## 三列

```html
<div class="three-col">
  Lorem ipsum dolor sit amet consectetur adipisicing elit. Eos pariatur sunt
  consequatur vitae corrupti dolores praesentium sapiente dolorum laudantium
  dolor ut natus, cum animi, cumque molestias libero eius nobis! Quod, veniam.
  Velit quo quos repudiandae minus est quaerat recusandae maiores impedit labore
  asperiores numquam amet, libero in dolorem cupiditate! Repudiandae impedit
  corporis sapiente sint velit similique, voluptatum cum nemo est magnam non
  ratione aliquam adipisci error perspiciatis voluptas quos dicta voluptatibus
  suscipit nam. Ad labore nihil sint quisquam eum corrupti nam. Explicabo,
  commodi. Labore molestias reiciendis vitae eos, iste possimus odio, voluptates
  quasi nam, voluptate est? Odio architecto doloribus quia.
</div>
```

CSS 如下：

```css
.three-col {
  column-count: 3;
  column-gap: 20px;
  column-rule: 1px dashed #ccc;
}
```

## 导航栏

```css
.nav {
  columns: 100px 4;
  column-gap: 0;
  column-rule: 1px solid #1a242f;
  background: #2c3e50;
}

.nav a {
  display: block;
  padding: 1em;
  border-bottom: 1px solid #1a242f;
  color: white;
  text-align: center;
  text-decoration: none;
}

.nav a:hover {
  background: #1a242f;
}
```

> [查看效果](https://codepen.io/lio-zero/pen/zYNROYG)

你可以查阅 [When And How To Use CSS Multi-Column Layout](https://www.smashingmagazine.com/2019/01/css-multiple-column-layout-multicol/) 以了解更多关于这方面的内容。
