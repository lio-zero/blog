# CSS background 属性

[`background`](https://developer.mozilla.org/en-US/docs/Web/CSS/background) 是一个 CSS 简写属性，用于设置任何元素的背景效果。它控制元素内容下方的绘制内容。

以下示例显示了可指定为元素创建任何背景效果的属性的完整列表：

```css
header {
  background: color image positionX positionY / size repeat attachment origin
    clip;
  background: plum /* color */ url('image/logo.jpg') /* image */ top center /
    /* position */ 150px 150px /* size */ no-repeat /* repeat */ padding-box
    /* origin */ content-box /* clip */ fixed; /* attachment */
}
```

`background` 简写形式由八个属性组成。让我们深入了解它们每一个的用法。

## `background-color`

CSS `background-color` 属性用于设置元素背景的颜色。如果图像需要时间加载，则可以将其与 `background-image` 属性组合作为备用选项。

`background-color` 值指定为颜色名称、十六进制值、RGB/RGBA 值或 HSL/HSLA 值。

### 默认值

`background-color` 的默认值是 `transparent` 关键字。

```css
header {
  background-color: transparent;
}
```

### 用法

```css
/* 关键字值 */
header {
  background-color: red;
}

/* 十六进制值 */
header {
  background-color: #bbff00;
}

/* RGB 值 */
header {
  background-color: rgb(255, 255, 128); /* 完全不透明 */
}
header {
  background-color: rgba(117, 190, 218, 0.5); /* 50% 透明 */
}

/* HSL 值 */
header {
  background-color: hsl(50, 33%, 25%); /* 完全不透明 */
}
header {
  background-color: hsla(50, 33%, 25%, 0.75); /* 75% 透明 */
}
```

## `background-image`

`background-image`是一个 CSS 属性，用于将一个或多个图像设置为元素的背景。当指定多个图像时，它们显示为一个堆栈，一个在另一个之上。

### 默认值

`background-image` 的默认值是 `none`，这意味着不会显示任何图像。

```css
header {
  background-image: none;
}
```

### 用法

此示例仅指定单个图像。

```css
header {
  background-image: url('test.jpg');
}
```

此示例将在 `second` 图像的顶部显示 `first` 图像。

```css
header {
  background-image: url('image/first.jpg'), url('image/second.jpg');
}
```

此示例指定 `banner` 图像上方的线性渐变。

```css
header {
  background-image: linear-gradient(
      to bottom,
      rgba(255, 255, 0, 0.5),
      rgba(0, 0, 255, 0.5)
    ), url('image/banner.jpg');
}
```

## `background-position`

`background-position` 属性设置背景图像的起始位置。其值指定图像相对于元素边框的放置的坐标。

`background-position` 可以用关键字指定（`left`、`top`、`bottom`、`right`、`center`）；百分比（`0%`，`25%` 等）；或长度（`0cm`，`3px` 等）。这些值可以组合在一起。

### 默认值

`background-position` 的默认值是 `0% 0%`，表示元素边框的左上角。

```css
header {
  background-position: 0% 0%; /* top left */
}
```

### 用法

```css
header {
  background-position: top; /* 顶部居中 */
  background-position: left; /* 左侧居中 */
  background-position: center; /* 水平垂直居中 */
  background-position: 20% 50%; /* 距左侧20% 距顶部50% */
  background-position: bottom 20px left 50px; /* 距底部20% 距左侧50% */
  background-position: left 2cm bottom 5cm; /* 距左侧2cm 距底部5cm */
}
```

## `background-size`

`background-size` 属性指定背景图像的大小。它定义了图像是被拉伸、以其原始大小显示还是填充整个可用空间。

值得注意的是，现在图像大小覆盖的空间将填充背景色。

### 关键字

`background-size` 可以使用关键字、百分比或长度指定。

- `cover`：将图像缩放到尽可能大的位置，但不会拉伸图像。它还可以垂直或水平裁剪图像以覆盖任何空白区域。
- `contain`：将图像缩放到尽可能大的位置，但不会裁剪或拉伸图像。
- `auto`：按相应方向缩放图像，使图像保持其原始比例

### 默认值

`background-size` 的默认值是 `auto`。

```css
header {
  background-size: auto;
}
```

### 用法

```css
header {
  background-size: auto /* 默认值 */ | cover /* 拉伸以填充元素的框 */ | contain; /* 缩放图像而不剪切/剪切。图像可以重复以填充元素的框 */
}

/* 按 20% 宽度缩放图像，并自动调整高度。图像可以重复以填充元素的框 */
header {
  background-size: 20% auto;
}

/* 按 3em 宽和 25% 高缩放图像。图像可以重复以填充元素的框 */
header {
  background-size: 3em 25%;
}

/* 可以为多个背景指定更多值 */
header {
  background-size: 50%, 25%, 60%;
}
```

## `background-repeat`

`background-repeat` 属性定义背景图像的重复方式。背景图像可以沿着水平、垂直、水平垂直重复，或者根本不重复。

由于默认情况下图像被剪裁为其元素的大小，因此也可以使用 `background-repeat` 属性缩放图像以适应或填充元素的区域。

### 关键字

`background-repeat` 属性是使用关键字值指定的。

- `repeat`：重复图像以覆盖整个背景
- `no-repeat`: 不会重复图片
- `space`：重复图像而不剪裁，并在重复图像之间均匀分布空白
- `round`：重复图像，并填充重复图像之间的空白
- `repeat-x`：仅沿 x 轴重复图像
- `repeat-y`：仅沿 y 轴重复图像

### 默认值

`background-repeat` 的默认值就是 `repeat`，它会重复图像，直到图像填充整个元素背景。

```css
header {
  background-repeat: repeat;
}
```

### 用法

```css
header {
  background-repeat: repeat-x /* 相当于 repeat no-repeat */ | repeat-y
    /* 相当于 no-repeat repeat */ | repeat /* 相当于 repeat repeat */ | space
    /* 相当于 space space */ | round /* 相当于 round round */ | no-repeat; /* 相当于 no-repeat no-repeat */
}
```

## `background-attachment`

`background-attachment` 属性指定背景图像应可滚动还是固定到视口。

### 关键字

`background-attachment` 属性仅使用关键字指定。

- `scroll`：将背景图像固定到其元素。不滚动包含内容的背景图像
- `fixed`：将背景图像固定到视口。即使内容可滚动，背景图像也不会滚动
- `local`：修复相对于元素内容的背景图像，从而允许背景图像随着内容滚动而滚动

### 默认值

`background-attachment` 的默认值为 `scroll`。

```css
header {
  background-attachment: scroll;
}
```

### 用法

```css
header {
  background-attachment: scroll
    /* 将图像固定到元素（仅当元素正在滚动而不是元素的内容时才能滚动） */ | fixed
    /* 将图像固定到视口 */ | local; /* 滚动包含内容的图像 */
}

/* 在多个图像上，我们可以指定双值 */
header {
  background-image: url('pic1.gif'), url('pic2.gif');
  background-attachment: fixed, scroll;
}
```

## `background-origin`

`background-origin` 属性指定在何处绘制背景图像。使用 `content-box` 值将跨越整个元素，在边框内使用 `border-box` 值或在填充内使用 `padding-box` 值。

**请注意**，当 `background-attachment` 设置为 `fixed` 时，`background-origin` 将被忽略，并且 `background-origin` 与 `background-clip` 相同，只是它调整背景的大小而不是剪辑。

### 关键字

`background-origin` 属性是使用关键字值指定的。

- `content-box`：相对于内容区域定位背景
- `border-box`：相对于边框区域定位背景
- `padding-box`：相对于填充区域定位背景

### 默认值

`background-origin` 的默认值是 `padding-box` 关键字。

```css
header {
  background-origin: padding-box;
}
```

### 用法

```css
header {
  background-origin: border-box | padding-box | content-box;
}

/* 我们可以为多个图像指定双值 */
header {
  background-image: url('logo.jpg'), url('banner.png');
  background-origin: content-box, padding-box;
}
```

## `background-clip`

`background-clip` 属性用于设置元素的背景（背景图片或颜色）是否延伸到边框、内边距盒子、内容盒子下面。

> **请注意**，此属性仅在指定了背景图像或背景颜色时才具有实际效果；否则，它将仅在边界上具有视觉效果。

### 关键字

`background-clip` 属性是使用关键字值指定的。

- `border-box`：默认值，将背景延伸到元素的边框区域外沿
- `content-box`：将背景裁剪至内容区域外沿。
- `padding-box`：将背景延伸到元素的填充区域外沿
- `text`：将背景被裁剪成文字的前景色。（**实验值**）

### 默认值

`background-clip` 的默认值是 `border-box` 关键字。

```css
header {
  background-clip: border-box;
}
```

### 用法

```css
header {
  background-clip: border-box | padding-box | content-box;
}
```
