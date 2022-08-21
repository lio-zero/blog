# JavaScript 媒体查询

首先，在 CSS3 中引入的媒体查询构成了响应式 Web 设计的核心组件。应用程序应根据每种类型设备（如手机、平板电脑、笔记本电脑、台式计算机）的限制进行定制，媒体查询提供了一种根据正在查看应用程序的设备大小设置视口尺寸的简便方法。

媒体查询允许您根据屏幕大小不同的视口尺寸，还可以帮助您为不同的设备不同的样式属性，包括设置方案、字体样式、动画、边框和动画，以及您认识的任何其他 CSS 属性。

一些前端开发人员可能会忽略了一个事实，即 JavaScript 也支持媒体查询。虽然不像 CSS 媒体查询那样流行，但 JavaScript 媒体查询提供了灵活性和许多优势，可以使它们成为某些用例的更好选择。

## JavaScript 媒体查询的好处

我们首先需要知道使用 JS 媒体查询的好处有哪些。

JavaScript 媒体查询有以下两个主要优势：

- **灵活性** — 您可以[通过编程方式将媒体查询](https://developer.mozilla.org/en-US/docs/Web/CSS/Media_Queries/Testing_media_queries)合并到 JavaScript 代码中，以便它们仅在特定事件开始时或满足特定条件时触发。使用仅限 CSS3 的方法，媒体查询描述的更改将对每个屏幕大小调整事件生效。
- **方便性** — JavaScript 媒体查询与使用 CSS 相同的语法。

**考虑一下：如果你想动态改变不同屏幕尺寸的属性，你会怎么做？**

你可能正在挠头，坚持这样做会很好：

```js
// 我们希望根据屏幕大小的变化执行的函数
function foo() {
  if (window.innerWidth < 1024) {
    /* TODO */
  }
}

// 设置一个监听器
window.addEventListener('resize', foo)
```

在上面的代码块中，我们有一个 `if` 语句，该语句基于小于 **1024** 的 `window.innerWidth`（即桌面显示的标准屏幕大小）。据代码推测，该方法应该在应用程序在比台式计算机小的设备上运行。

不幸的是，这种方法成本高昂，因为它会在每次调整大小时触发，而不仅仅是在用户在手机或平板电脑上打开应用程序时触发。没错，用户在桌面计算机上手动调整屏幕大小时，此方法将随时运行。过多的此类操作最终会导致应用程序延迟。

值得庆幸的是，我们有完美的 API 来处理动态情况和响应设计：[`matchMedia API`](https://developer.mozilla.org/en-US/docs/Web/API/Window/matchMedia)。

基本上所有的浏览器都支持 `matchMedia`，以下是 [Can I Use](https://caniuse.com/?search=matchMedia) 给出的支持情况：

![matchMedia 的支持情况](https://upload-images.jianshu.io/upload_images/18281896-fa997ad418662f50.image?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

如果你想支持更低版本的浏览器，这里有个 [`polyfill`](https://github.com/paulirish/matchMedia.js/) 可以帮助到您。

## 如何使用 JavaScript 媒体查询

我们可以使用 `matchMedia API`，而不是像在上面的示例中那样将监听器附加到 `resize` 事件。

`matchMedia()` 方法本质上是将监听器附加到媒体查询，但不会对窗口或屏幕大小的每一个更改做出响应，从而显著提高性能。如果我们利用这种方法，我们只负责开发我们希望为屏幕调整执行的逻辑，而不必担心其他条件、验证和代码优化。

要使用此 API，我们可以调用 `window.matchMedia()` 并传入一个媒体查询字符串，指定要响应的屏幕大小。

```js
// 此媒体查询以最小宽度为 320px 的视口为目标
const mQuery = window.matchMedia('(min-width: 320px)')
```

`matchMedia()` 方法返回一个新的 [`MediaQueryList` 对象](https://developer.mozilla.org/en-US/docs/Web/API/MediaQueryList)，我们在上面的示例中将其命名为 `mQuery`。此对象存储有关应用于特定文档的媒体查询的信息，以及支持事件驱动和即时匹配的方法。这允许我们在 `resize` 事件开始时触发自定义逻辑。

要执行必要的调整大小逻辑，我们需要检查 [`window.matches`](https://developer.mozilla.org/en-US/docs/Web/API/MediaQueryList/matches)，这是一个布尔属性，如果媒体查询匹配，则返回 `true`。如果不匹配，则返回 `false`。在我们的示例中，此属性告诉我们是否存在与指定条件的实际匹配（即屏幕的最小宽度为 320px）。

```js
// 检查媒体查询是否已匹配
if (mQuery.matches) {
  console.log('媒体查询匹配！')
}
```

可以看到，使用起来很简单。现在，只有一个问题：`window.matches` 只能执行此检查一次。为了促进响应性 web 设计，我们希望不断检查正在发生的任何更改。辛运的是，还有另一个方法可以与 `windows.matches` 配合使用来帮助我们实现这一点：[`addListener()`](https://developer.mozilla.org/en-US/docs/Web/API/MediaQueryList/addListener) 方法。

## `addListener()` 方法

`matchMedia API` 提供了 `addListener()` 方法以及相应的 [`removeListener()`](https://developer.mozilla.org/en-US/docs/Web/API/MediaQueryList/removeListener) 方法。调用 `addListener()` 时，我们传入一个回调函数，该函数在检测到媒体查询匹配状态发生更改时运行。此回调函数是我们希望在 `resize` 事件中触发的函数：

```js
// 此媒体查询以最小宽度为 320px 的视口为目标
const mQuery = window.matchMedia('(min-width: 320px)')

function handleMobilePhoneResize(e) {
  // 检查媒体查询是否为真
  if (e.matches) {
    console.log('媒体查询匹配！')
  }
}

mQuery.addListener(handleMobilePhoneResize)
```

这种技术允许我们响应媒体查询更改，并根据需要动态调用其他方法。然后，这些动态调用的方法可以更改各种文档属性，如字体样式、边框和间距、动画等。

例如，为 😊 emoji 添加动画，我们可以通过以下方式实现：

```js
const reduceMotionQuery = matchMedia('(min-width: 320px)')
reduceMotionQuery.addListener(setBlushAnimate)

function setBlushAnimate() {
  if (reduceMotionQuery.matches) {
    document.body.style.setProperty('--toggle', '0')
    document.body.style.setProperty('--playState', 'paused')
  } else {
    document.body.style.setProperty('', '1')
    document.body.style.setProperty('--playState', 'running')
  }
}

setBlushAnimate()
```

效果如下：

![😊 动起来](https://upload-images.jianshu.io/upload_images/18281896-75b066233e6a5ad8.image?imageMogr2/auto-orient/strip)

> [查看效果](https://codepen.io/lio-zero/pen/OJmqaPO)

## 最后

现在，您应该对 JavaScript 中的媒体查询以及它们如何实现高效、自适应的设计有了基本的了解。`matchMedia API` 可以帮助您使用 JavaScript 创建媒体查询，`addListener()` 可以通过调用回调函数来构建响应性跨平台体验。

定期轮询文档状态更改是一种低效且资源密集的方法，最终会导致应用程序延迟。使用 `matchMedia()` 可以观察特定文档，检测媒体查询条件中的更改，并根据媒体查询状态以编程方式更改文档属性。

## 更多资料

[Working with JavaScript Media Queries](https://css-tricks.com/working-with-javascript-media-queries/)
