# Web Fullscreen API

[`Fullscreen API`](https://developer.mozilla.org/en-US/docs/Web/API/Fullscreen_API) 可以使用你以全屏模式打开或关闭元素。例如，允许 `video` 或 `canvas` 元素，使其占据整个屏幕的。

你还需要知道：

- 全屏显示可以是任意元素
- HTML5 API 存在兼容性问题，即使高版本浏览器也有兼容性问题
- 不同浏览器需要添加不同的前缀 `webkit`、`moz`、`ms`

![Fullscreen API 的兼容情况](https://upload-images.jianshu.io/upload_images/18281896-3b7fe2b9171715d7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 基本用法

主要了解它的两个方法：

- `document.requestFullscreen()` — 在系统上以全屏模式显示选定的元素，从而关闭其他应用程序以及浏览器和系统 UI 元素。
- `document.exitFullscreen()` — 将全屏模式退出到正常模式。

```js
const fullscreen = (mode = true, el = 'body') =>
  mode
    ? document.querySelector(el).requestFullscreen()
    : document.exitFullscreen()

fullscreen() // 默认全屏模式打开 "body"
fullscreen(false) // 退出全屏模式
```

CSS 还提供了一个 `:fullscreen` 伪元素，当浏览器在全屏模式下时应用。

```css
.elem:fullscreen {
  background-color: #e4708a;
  width: 100vw;
  height: 100vh;
}
```

## 兼容

并不是所有的浏览器都支持 Fullscreen API，你可以做一下适当的兼容来解决问题。

```js
function launchFullScreen(elem) {
  if (elem.requestFullScreen) {
    elem.requestFullScreen()
  } else if (elem.mozRequestFullScreen) {
    elem.mozRequestFullScreen()
  } else if (elem.webkitRequestFullScreen) {
    elem.webkitRequestFullScreen()
  } else if (elem.msRequestFullscreen) {
    elem.msRequestFullscreen()
  } else {
    elem.oRequestFullScreen()
  }
}
```

对应的，也需要添加 CSS 前缀。**注意**：有些使用旧的 `:full-screen` 语法而不是标准 `:fullscreen`。

```css
div:-webkit-full-screen {
}
:-moz-full-screen {
}
:-ms-fullscreen {
}
```

## 更多资料

[screenfull](https://github.com/sindresorhus/screenfull) — 跨浏览器使用 JavaScript Fullscreen API 的简单包装器。

它的源码很简单，可以阅读一下 👉 [地址](https://github.com/sindresorhus/screenfull/blob/main/index.js)。
