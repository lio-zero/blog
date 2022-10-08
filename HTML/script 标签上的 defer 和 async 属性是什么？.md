# script 标签上的 defer 和 async 属性是什么？

我们经常使用 `script` 标签向页面插入一个常规的 JavaScript 文件：

```html
<script src="/path/to/script.js"></script>
```

当浏览器看到普通脚本标签声明时，它将执行以下步骤：

- 暂停 HTML 文档解析器
- 创建新请求以下载脚本
- 在脚本完全下载后立即执行脚本
- 执行结束后，继续解析 HTML 文档

![解析过程](https://upload-images.jianshu.io/upload_images/18281896-f73699ab4fde8302.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

此流程提供了糟糕的用户体验，假如脚本下载时间过长，用户将无法与页面交互。在看到整个页面被解析之前，他们必须等待所有脚本被下载并完全执行。

为了解决这个问题，HTML5 为 `script` 标签提供了两个属性。它们是 `async` 和 `defer`：

```html
<script src="/path/to/script.js" async></script>
<script src="/path/to/script.js" defer></script>
```

**`async` 和 `defer` 的作用是**让浏览器知道脚本可以与文档解析器过程并行下载，从而不阻塞页面的渲染。

但是，它们之间也存在一些本质的区别。

## 区别

`async` 和 `defer` 脚本在不同的时刻执行。

下载 `async` 脚本后，浏览器将暂停文档解析器，执行脚本并继续解析文档。

- 解析文档
- 下载脚本
- 暂停解析
- 执行脚本
- 恢复解析

另一方面，只有当解析器完成其工作时，才会执行 `defer` 脚本。

- 解析文档
- 下载脚本
- 执行脚本

> **注意**：这个过程文档是不会停止解析的。

`async` 脚本在完全下载后立即执行，加载和渲染后续文档元素的过程将和 JS 脚本的加载与执行并行进行（异步），因此它们的执行顺序可能与页面中显示的顺序不同（这个时候操作 DOM 可能会获取不到）。

另一方面，`defer` 脚本**保证执行顺序**。它是等到页面渲染完毕，所有脚本下载完成，在 `DOMContentLoaded` 事件前按照脚本的在文档中的顺序执行。

```html
<script src="longTime.js"></script>
<script src="longTime.js" defer></script>
<script src="longTime.js" async></script>
```

> **注意**：没有 `src` 属性的脚本（即不是内联脚本），`async` 和 `defer` 属性会被忽略。

## 尽可能使用 `async` 和 `defer`

确保 JavaScript 脚本兼容 `async` 和 `defer`，任何时候都要尽可能使用 `async`，特别是当你有较多的 `script` 标签时。

这样在加载 JavaScript 的过程中页面就不会重新绘制，否则，浏览器在不具有这些特性的 `script` 标签后就不会重绘任何东西。

> [消除渲染阻塞资源](https://web.dev/render-blocking-resources/)

## 一些好的做法

对不依赖于其他脚本的脚本使用 `async`。统计脚本（例如：[百度统计](https://tongji.baidu.com/)或 [Google Analytic](https://analytics.google.com/)）将适合此用例。

通常，将脚本放在 `</body>` 之前。

```html
<body>
  <!-- ... -->
  <script src="/path/to/script.js"></script>
  <script src="/path/to/script.js"></script>
  <script src="/path/to/script.js"></script>
</body>
```

默认情况下，注入页面动态的脚本是异步的。但是，如果需要，可以将 `async` 属性设置为 `false`。

```js
const script = document.createElement('script')
script.src = '/path/to/script.js'
script.async = false
document.head.appendChild(script)
```

## 总结

- 关于 `defer`，我们还要记住的是它是按照加载顺序执行脚本的
- 标记为 `async` 的脚本并不保证按照指定它们的先后顺序执行。对它来说脚本的加载和执行是紧紧挨着的，所以不管你声明的顺序如何，只要它加载完了就会立刻执行。
- `async` 对于应用脚本的用处不大，因为它完全不考虑依赖（哪怕是最低级的顺序执行），不过它对于那些可以不依赖任何脚本或不被任何脚本依赖的脚本来说却是非常合适的。

## 更多资料

[脚本：async，defer](https://zh.javascript.info/script-async-defer)
