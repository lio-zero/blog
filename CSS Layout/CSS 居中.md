# CSS 居中

基本结构如下：

```html
<div class="container">
  <div class="box"></div>
</div>
```

基本样式如下：

```css
.container {
  width: 200px;
  height: 200px;
  background-color: pink;
}

.box {
  width: 50px;
  height: 50px;
  background-color: plum;
}
```

## table-cell

```css
.container {
  display: table-cell;
  vertical-align: middle;
}

.box {
  margin: auto;
}
```

或者：

```css
.container {
  display: table-cell;
  text-align: center;
  vertical-align: middle;
}

.box {
  display: inline-block;
}
```

## position

**未知宽度和高度的元素**，可以使用绝对定位和 `transform` 属性的 `translate`。

```css
.container {
  position: relative;
}

.box {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}
```

`transform` 还可以使用 `margin` 代替，但需要提前知道元素的宽高：

```css
.container {
  position: relative;
}

.box {
  position: absolute;
  top: 50%;
  left: 50%;
  margin: -25px 0 0 -25px; // width 和 height 的负一半
}
```

另一种方式，设置 `absolute` 的偏移值（`top`、`left`、`right` 和 `bottom`）为 `0`，并使用 `margin: auto`，好处是不需要提前知道元素的尺寸兼容性好。

```css
.container {
  position: relative;
}

.box {
  position: absolute;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  margin: auto;
}
```

## Flex

使用 `justify-content` 和 `align-item`：

```css
.container {
  display: flex;
  justify-content: center;
  align-items: center;
}
```

使用 `justify-content` 和 `align-self`：

```css
.container {
  display: flex;
  justify-content: center;
}

.box {
  align-self: center;
}
```

使用 `margin: auto`：

```css
.container {
  display: flex;
}

.box {
  margin: auto;
}
```

## 使用 Grid 居中

`grid` 用于二维布局，但当只有一个子元素时，一维布局与二维布局都一样。

```css
.container {
  display: grid;
  justify-content: center;
  align-content: center;
}
/* or */
.container {
  display: grid;
  justify-items: center;
  align-items: center;
}
```

上面属性的简写形式：

```css
.container {
  display: grid;
  place-items: center;
}
/* or */
.container {
  display: grid;
  place-content: center;
}
```

更简便的技巧：

```css
.container {
  display: grid;
}

.box {
  margin: auto;
}
```

另一种方式，我们可以单独控制子元素的对齐方式：

```css
.container {
  display: grid;
}

.box {
  align-self: center;
  justify-self: center;
  /* 简写形式 */
  /* place-self: center; */
}
```

搭配众多，一个个尝试。

> [演示地址](https://codepen.io/lio-zero/pen/XWVyqjZ)

## 更多资料

- [Centering in CSS: A Complete Guide](https://css-tricks.com/centering-css-complete-guide/)
- [How to horizontally center an element](https://stackoverflow.com/questions/114543/how-to-horizontally-center-an-element)
