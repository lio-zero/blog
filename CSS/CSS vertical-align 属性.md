# CSS vertical-align 属性

CSS [`vertical-align`](https://developer.mozilla.org/en-US/docs/Web/CSS/vertical-align) 属性控制在一行上相邻设置的元素如何对齐。

```css
img {
  vertical-align: middle;
}
```

> **注意**：为了使其正常工作，我们需要沿基线设置元素。如：`inline` 内联元素（例如 `<span>`，`<img>`）或 `inline-block` 内联块（例如，由 `display` 属性设置）元素。`vertical-align` 属性也适用于 `table-cell` 元素。

## 语法

```css
elem {
  vertical-align: baseline | top | bottom | middle | text-top | text-bottom |
    sub | super | length units;
}
```

## 关键字

- `vertical-align` — 默认值。顾名思义，它将元素与父元素的基线对齐。
- `top` — 将元素与一行中最高元素的顶部对齐。
- `bottom` — 将元素与底部对齐，元素处于同一级别。
- `middle` — 将元素与其父元素的中心对齐。
- `text-top` — 使用其父元素行中最高字体的顶部对齐元素。
- `text-bottom` — 使用其父元素行中最高字体的底部对齐元素。
- `sub` — 将元素对齐到其父元素的基线下标。它的行为更像 `<sub>` 标签。
- `super` — 将元素与父元素的基线上标对齐。它的行为更像 `<sup>` 标签。

## 长度和百分比值

将元素与给定单位对齐。正数将使元素与基线上方对齐，负值将使元素与基线下方对齐。

这些值可以是任意长度单位：`px`，`em`，`%`，等。

```css
img {
  vertical-align: 10px;
}

img {
  vertical-align: 50%;
}

img {
  vertical-align: 3em;
}
```

## 全局值

`initial` — 将元素的对齐方式设置为其默认值，即 `baseline`。

```css
img {
  vertical-align: initial;
}
```

`inherit` — 将元素的对齐方式设置为其父元素的值。

```css
img {
  vertical-align: inherit;
}
```

## 例子

### 表格

`vertical-align` 属性可以直接应用用于表格单元格，可以将对齐单元格内的内容。重要的一点是，它能很好的兼容浏览器在显示效果上的一致性。

```css
td {
  height: 40px;
  vertical-align: middle;
}
```

效果如下：

![vertical-align](https://upload-images.jianshu.io/upload_images/18281896-61ce21993fb07bdb.image?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 垂直居中

`vertical-align` 属性不允许您在另一个元素中 “垂直居中” 一个元素。我们更多的会使用 **Flexbox** 来做垂直居中。

但是，您可能不知道，有一个 **ghost** 技巧可以帮助您垂直居中一个元素。

```css
p {
  border: 1px solid plum;
  display: table;
  height: 100px;
  width: 100px;
}

span {
  display: table-cell;
  vertical-align: middle;
}
```

效果如下：

![vertical-align](https://upload-images.jianshu.io/upload_images/18281896-a45337c39b47f83e.image?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
