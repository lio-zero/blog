# 常用的一些 CSS 技巧三

你可以看看其他常用的 CSS 技巧：

- [常用的一些 CSS 技巧一](https://github.com/lio-zero/blog/blob/master/CSS/%E5%B8%B8%E7%94%A8%E7%9A%84%E4%B8%80%E4%BA%9B%20CSS%20%E6%8A%80%E5%B7%A7%E4%B8%80.md)
- [常用的一些 CSS 技巧二 — 选择器（伪类与伪元素）](https://github.com/lio-zero/blog/blob/master/CSS/%E5%B8%B8%E7%94%A8%E7%9A%84%E4%B8%80%E4%BA%9B%20CSS%20%E6%8A%80%E5%B7%A7%E4%BA%8C%20%E2%80%94%20%E9%80%89%E6%8B%A9%E5%99%A8%EF%BC%88%E4%BC%AA%E7%B1%BB%E4%B8%8E%E4%BC%AA%E5%85%83%E7%B4%A0%EF%BC%89.md)

以下整理了 [30 seconds of code](https://www.30secondsofcode.org/) 的一些简短 CSS 代码段。

## Offscreen

在 DOM 中在视觉上和位置上完全隐藏元素，同时仍允许对其进行访问。

**注意**：这为 `display: none`（屏幕阅读器无法读取）和 `visibility: hidden`（占用 DOM 中的物理空间）提供了一种可访问且布局友好的替代方案。

```html
<a class="button" href="https://google.com">
  Learn More <span class="offscreen"> about pants</span>
</a>
```

CSS 如下：

```css
.offscreen {
  border: 0;
  clip: rect(0 0 0 0);
  height: 1px;
  margin: -1px;
  overflow: hidden;
  padding: 0;
  position: absolute;
  width: 1px;
}
```

## 汉堡按钮

显示汉堡包菜单，该菜单在悬停时转换为十字按钮。

```html
<div class="hamburger-menu">
  <div class="bar top"></div>
  <div class="bar middle"></div>
  <div class="bar bottom"></div>
</div>
```

CSS 如下：

```css
.hamburger-menu {
  display: flex;
  flex-flow: column wrap;
  justify-content: space-between;
  height: 2.5rem;
  width: 2.5rem;
  cursor: pointer;
}

.hamburger-menu .bar {
  height: 5px;
  background: black;
  border-radius: 5px;
  margin: 3px 0px;
  transform-origin: left;
  transition: all 0.5s;
}

.hamburger-menu:hover .top {
  transform: rotate(45deg);
}

.hamburger-menu:hover .middle {
  opacity: 0;
}

.hamburger-menu:hover .bottom {
  transform: rotate(-45deg);
}
```

## CSS 缓动变量

大多数 web 开发人员在设计中使用内置的 `ease`、`ease-in`、`ease-out` 或 `ease-in-out` 函数来处理大多数 `transition-timing-function` 的用例。虽然这些都非常适合日常使用，但有一个更强大的选项，即 `bezier-curve()` 函数。
使用 `bezier-curve()` 我们可以轻松地定义自定义变量，帮助我们的设计脱颖而出。实际上，上面提到的内置函数也可以使用 `bezier-curve()` 函数编写。为了便于使用，这里提供了一些存储在 CSS 变量中的有用函数：

```css
:root {
  /* ease-in corresponds to cubic-bezier(0.42, 0, 1.0, 1.0) */
  --ease-in-quad: cubic-bezier(0.55, 0.085, 0.68, 0.53);
  --ease-in-cubic: cubic-bezier(0.55, 0.055, 0.675, 0.19);
  --ease-in-quart: cubic-bezier(0.895, 0.03, 0.685, 0.22);
  --ease-in-quint: cubic-bezier(0.755, 0.05, 0.855, 0.06);
  --ease-in-expo: cubic-bezier(0.95, 0.05, 0.795, 0.035);
  --ease-in-circ: cubic-bezier(0.6, 0.04, 0.98, 0.335);

  /* ease-out corresponds to cubic-bezier(0, 0, 0.58, 1.0) */
  --ease-out-quad: cubic-bezier(0.25, 0.46, 0.45, 0.94);
  --ease-out-cubic: cubic-bezier(0.215, 0.61, 0.355, 1);
  --ease-out-quart: cubic-bezier(0.165, 0.84, 0.44, 1);
  --ease-out-quint: cubic-bezier(0.23, 1, 0.32, 1);
  --ease-out-expo: cubic-bezier(0.19, 1, 0.22, 1);
  --ease-out-circ: cubic-bezier(0.075, 0.82, 0.165, 1);

  /* ease-in-out corresponds to cubic-bezier(0.42, 0, 0.58, 1.0) */
  --ease-in-out-quad: cubic-bezier(0.455, 0.03, 0.515, 0.955);
  --ease-in-out-cubic: cubic-bezier(0.645, 0.045, 0.355, 1);
  --ease-in-out-quart: cubic-bezier(0.77, 0, 0.175, 1);
  --ease-in-out-quint: cubic-bezier(0.86, 0, 0.07, 1);
  --ease-in-out-expo: cubic-bezier(1, 0, 0, 1);
  --ease-in-out-circ: cubic-bezier(0.785, 0.135, 0.15, 0.86);
}
```

## 顶部三角形的边框

创建一个内容容器，其顶部为三角形。

```css
.container {
  width: 200px;
  position: relative;
  background: #ffffff;
  padding: 15px;
  border: 1px solid #dddddd;
  border-top: 2px solid #ffd980;
  margin-top: 20px;
}
.container:before,
.container:after {
  content: '';
  position: absolute;
  bottom: 100%;
  left: 18px;
  border: 12px solid transparent;
  border-bottom-color: #ffd980;
}
.container:after {
  left: 20px;
  border: 10px solid transparent;
  border-bottom-color: #ffffff;
}
```

## 切换开关

仅使用 CSS 创建切换开关。

```css
.switch {
  position: relative;
  display: inline-block;
  width: 40px;
  height: 20px;
  background-color: rgba(0, 0, 0, 0.25);
  border-radius: 20px;
  transition: all 0.3s;
}

.switch:after {
  content: '';
  position: absolute;
  width: 18px;
  height: 18px;
  border-radius: 18px;
  background-color: white;
  top: 1px;
  left: 1px;
  transition: all 0.3s;
}

input[type='checkbox']:checked + .switch:after {
  transform: translateX(20px);
}

input[type='checkbox']:checked + .switch {
  background-color: #7983ff;
}

.offscreen {
  position: absolute;
  left: -9999px;
}
```

## 脉冲效果

使用 `animation-delay` 属性创建脉冲效果加载程序动画。

```html
<div class="ripple-loader">
  <div></div>
  <div></div>
</div>
```

CSS 如下：

```css
.ripple-loader {
  position: relative;
  width: 64px;
  height: 64px;
}

.ripple-loader div {
  position: absolute;
  border: 4px solid #454ade;
  border-radius: 50%;
  animation: ripple-loader 1s ease-out infinite;
}

.ripple-loader div:nth-child(2) {
  animation-delay: -0.5s;
}

@keyframes ripple-loader {
  0% {
    top: 32px;
    left: 32px;
    width: 0;
    height: 0;
    opacity: 1;
  }
  100% {
    top: 0;
    left: 0;
    width: 64px;
    height: 64px;
    opacity: 0;
  }
}
```

## 纯 CSS 水平滚动

创建一个可水平滚动的容器，滚动时该容器将捕捉到元素。

```html
<div class="horizontal-snap">
  <a href="#"><img src="https://picsum.photos/id/1067/640/640" /></a>
  <a href="#"><img src="https://picsum.photos/id/122/640/640" /></a>
  <a href="#"><img src="https://picsum.photos/id/188/640/640" /></a>
  <a href="#"><img src="https://picsum.photos/id/249/640/640" /></a>
  <a href="#"><img src="https://picsum.photos/id/257/640/640" /></a>
  <a href="#"><img src="https://picsum.photos/id/259/640/640" /></a>
</div>
```

CSS 如下：

```css
.horizontal-snap {
  margin: 0 auto;
  display: grid;
  grid-auto-flow: column;
  gap: 1rem;
  height: calc(180px + 1rem);
  padding: 1rem;
  width: 480px;
  overflow-y: auto;
  overscroll-behavior-x: contain;
  scroll-snap-type: x mandatory;
}

.horizontal-snap > a {
  scroll-snap-align: center;
}

.horizontal-snap img {
  width: 180px;
  max-width: none;
  object-fit: contain;
  border-radius: 1rem;
}
```

## CSS 渐变

[uiGradients](https://uigradients.com/) 包含了非常丰富的现成 CSS 渐变集合，可用于几乎所有内容。

```css
.argon {
  background: linear-gradient(to right, #03001e, #7303c0, #ec38bc, #fdeff9);
}

.celestial {
  background: linear-gradient(to right, #c33764, #1d2671);
}
```

你也可以在 [Web Gradients](https://webgradients.com/) 找到你想要的渐变色彩，它提供 180 个线性渐变的免费集合，您可以将其用作网站任何部分的内容背景。

## 恒定的宽高比

确保具有可变 `width` 的元素将保留一个比例的 `height` 值。

```css
.constant-width-to-height-ratio {
  background: #9c27b0;
  width: 50%;
}

.constant-width-to-height-ratio:before {
  content: '';
  padding-top: 100%;
  float: left;
}

.constant-width-to-height-ratio:after {
  content: '';
  display: block;
  clear: both;
}
```

## 渐变文字

使文本具有渐变颜色。

```css
.gradient-text {
  background: linear-gradient(#70d6ff, #00072d);
  -webkit-text-fill-color: transparent;
  -webkit-background-clip: text;
  font-size: 32px;
}
```

## 鼠标光标渐变跟踪

渐变跟随鼠标光标的悬停效果。

- 声明两个 CSS 变量 `--x` 和 `--y`，用于跟踪鼠标在按钮上的位置。
- 声明一个 CSS 变量，`--size` 用于修改渐变的尺寸。

- 在正确的位置创建渐变 `background: radial-gradient(circle closest-side, pink, transparent);`
- 使用 `document.querySelector()` 和 `EventTarget.addEventListener()` 注册 `'mousemove'` 事件的处理程序。
- 使用 `Element.getBoundingClientRect()` 和 `CSSStyleDeclaration.setProperty()` 更新 `--x` 和 `--y` 变量的值。

```js
let btn = document.querySelector('.mouse-cursor-gradient-tracking')
btn.addEventListener('mousemove', (e) => {
  let rect = e.target.getBoundingClientRect()
  let x = e.clientX - rect.left
  let y = e.clientY - rect.top
  btn.style.setProperty('--x', x + 'px')
  btn.style.setProperty('--y', y + 'px')
})
```

## 仅 CSS 图像预加载

```css
.grass {
  background: url(img/grass.png) no-repeat -9999px -9999px;
}

.grass:hover {
  background-position: bottom left;
}
```

## 图片文字叠加

使用覆盖图在图像顶部显示文本。

```html
<div>
  <h3 class="text-overlay">Hello, World</h3>
  <img src="https://picsum.photos/id/1050/1200/800" />
</div>
```

CSS 如下：

```css
div {
  position: relative;
}

.text-overlay {
  position: absolute;
  top: 0;
  left: 0;
  padding: 1rem;
  font-size: 2rem;
  font-weight: 300;
  color: white;
  backdrop-filter: blur(14px) brightness(80%);
}
```

## CSS 过渡的完美时机

我们都经历了网站互动，由于过渡或动画持续时间和时间安排不佳，因此感觉呆滞或不畅。但是，有一个非常简单的 "黄金法则" 可以帮助您避免这种糟糕的用户体验，称为 **Doherty Threshold**：

> 当计算机及其用户以一定的速度（<400 毫秒）进行交互时，生产力就将大大提高，从而确保彼此不必等待对方。

0.4s 的幻数听起来是一个非常合理的阈值，但是看看你最喜欢的网站，你会发现大多数的过渡持续时间或动画持续时间值都接近 0.3 秒，这可能与我们最近对快速交互的期望有关——在 1982 年所有相关的研究论文发表之后。

## 动画加载

创建一个弹跳的加载器动画。

```html
<div class="bouncing-loader">
  <div></div>
  <div></div>
  <div></div>
</div>
```

CSS 如下：

```css
.bouncing-loader {
  display: flex;
  justify-content: center;
}

.bouncing-loader > div {
  width: 16px;
  height: 16px;
  margin: 3rem 0.2rem;
  background: #8385aa;
  border-radius: 50%;
  animation: bouncing-loader 0.6s infinite alternate;
}

.bouncing-loader > div:nth-child(2) {
  animation-delay: 0.2s;
}

.bouncing-loader > div:nth-child(3) {
  animation-delay: 0.4s;
}

@keyframes bouncing-loader {
  to {
    opacity: 0.1;
    transform: translate3d(0, -16px, 0);
  }
}
```

## 悬停下划线动画

当文本悬停在上方时，创建动画的下划线效果。

```css
.hover-underline-animation {
  display: inline-block;
  position: relative;
  color: #0087ca;
}

.hover-underline-animation:after {
  content: '';
  position: absolute;
  width: 100%;
  transform: scaleX(0);
  height: 2px;
  bottom: 0;
  left: 0;
  background-color: #0087ca;
  transform-origin: bottom right;
  transition: transform 0.25s ease-out;
}

.hover-underline-animation:hover:after {
  transform: scaleX(1);
  transform-origin: bottom left;
}
```

## 弹出菜单

显示 hover/focus 上的交互式弹出菜单。

```css
.reference {
  position: relative;
  background: tomato;
  width: 100px;
  height: 80px;
}

.popout-menu {
  position: absolute;
  visibility: hidden;
  left: 100%;
  background: #9c27b0;
  color: white;
  padding: 16px;
}

.reference:hover > .popout-menu,
.reference:focus > .popout-menu,
.reference:focus-within > .popout-menu {
  visibility: visible;
}
```

## 按钮边框动画

在悬停时创建边框动画。

```css
.animated-border-button {
  background-color: #141414;
  border: none;
  color: #ffffff;
  outline: none;
  padding: 12px 40px 10px;
  position: relative;
}

.animated-border-button:before,
.animated-border-button:after {
  border: 0 solid transparent;
  transition: all 0.3s;
  content: '';
  height: 0;
  position: absolute;
  width: 24px;
}

.animated-border-button:before {
  border-top: 2px solid #141414;
  right: 0;
  top: -4px;
}

.animated-border-button:after {
  border-bottom: 2px solid #141414;
  bottom: -4px;
  left: 0;
}

.animated-border-button:hover:before,
.animated-border-button:hover:after {
  width: 100%;
}
```
