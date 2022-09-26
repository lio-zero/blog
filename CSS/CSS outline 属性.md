# CSS outline 属性

CSS [`outline`](https://developer.mozilla.org/en-US/docs/Web/CSS/outline)（轮廓）是一个简写属性，用于围绕元素外部绘制一条线。它与 `a:focus` 选择器结合使用特别有用，可以更加强调链接或其他元素。

`outline` 与 `border` 相似，不同之处在于 `outline` 在整个元素周围画了一条线；它不能像 `border` 那样，指定在元素的一个面上设置轮廓，也就是不能单独设置顶部轮廓、右侧轮廓、底部轮廓或左侧轮廓。

## 语法

```css
outline: width | style | color;
```

`outline` 简写属性可以用一个、两个或三个值声明，并且它们的顺序可以任意更换。例如：

```css
outline: solid; /* 这仅指定样式值 */

outline: dashed #eee; /* 这将指定颜色值和样式值 */

outline: thick inset; /* 这将指定宽度值和样式值 */

outline: 1px solid plum; /* 这将指定所有 3 个值 */
```

![outline](https://upload-images.jianshu.io/upload_images/18281896-4c29ac0c5fda6f5b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这里需要注意两点：

- 当仅指定一个或两个值时，其他值将解析为其默认值。`outline` 只需设置其 `style` 值即可工作。
- 如果未定义样式，许多元素的轮廓将不可见。这是因为样式默认为 `none`。一个例外是 `input` 元素，它们由浏览器赋予默认样式。

## 属性

`outline` 属性是由三个单独属性组成的缩写，用于定义轮廓的颜色、宽度和样式。我们将在下面逐一探讨。

### `outline-width`

`outline-width` 定义要绘制的线的厚度。其值可以是任意长度值，也可以是以下任意关键字：

- `thin`
- `medium`（默认值）
- `thick`

```css
outline-width: 2 (px, em, rem) | thin | medium | thick;
```

### `outline-style`

`outline-style` 定义要绘制的线的类型。它的值可以是以下任何关键字：

- `auto`
- `dotted`
- `dashed`
- `solid`
- `double`
- `groove`
- `ridge`
- `inset`
- `outset`
- `none`（默认值，没有轮廓）

```css
outline-style: auto | dotted | dashed | solid | double | groove | ridge | inset
  | outset;
```

### `outline-color`

`outline-color` 用于设置一个元素轮廓的颜色。它可以通过关键字、十六进制值、RGB/RGBA 值和 HSL/HSLA 值来指定。

如果[浏览器支持](https://caniuse.com/?search=outline-color)，其默认值为 `invert`；否则，其默认值为 [`currentColor`](https://github.com/lio-zero/css-tricks#%E4%BD%BF%E7%94%A8-currentcolor-%E5%85%B3%E9%94%AE%E5%AD%97%E9%87%8D%E7%94%A8%E5%BD%93%E5%89%8D%E9%A2%9C%E8%89%B2)。

```css
outline-color: currentColor | red | #eee | rgb(255, 255, 255) | hsl(0, 0, 0);
```

## 有趣的用法

尝试打开任何网站上的控制台并运行以下内容：

```js
document.head.insertAdjacentHTML(
  'beforeend',
  '<style>* { outline: 1px solid plum; }</style>'
)
```

你们会看到很多网站都是这样的结构：

![outline](https://upload-images.jianshu.io/upload_images/18281896-707c9f70b633ab55.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

默认情况下，`outline` 用于 `:focus` 样式。但请记住，如果删除轮廓样式，例如：`a:focus { outline: 0; }`，您需要使用其他视觉上不同的样式将它们添加回来。
