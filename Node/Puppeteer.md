# Puppeteer

[Puppeteer](https://pptr.dev/) 是 Chrome 开发团队在 2017 年发布的一个 Node.js 包，用来模拟 Chrome 浏览器的运行。

![Puppeteer logo](https://user-images.githubusercontent.com/10379601/29446482-04f7036a-841f-11e7-9872-91d1fc2ea683.png)

它提供了高级 API 来通过 [DevTools 协议](https://chromedevtools.github.io/devtools-protocol/)控制 Chrome 或 Chromium。Puppeteer 默认情况下[无头](https://developers.google.com/web/updates/2017/04/headless-chrome)运行，但可以配置为运行完整（无头）的 Chrome 或 Chromium。

使用它，我们可以：

- 抓取网页
- 自动化表单提交
- 执行任何类型的浏览器自动化
- 跟踪页面加载性能
- 创建单页应用的服务器端渲染版本
- 制作屏幕截图
- 创建自动化测试
- 从网页生成 PDF

它是由谷歌构建的。它本身并没有解锁任何新东西，但它抽象了我们在不使用它的情况下必须处理的许多细节。

简而言之，它使事情变得非常简单。

由于它在初始化时启动了一个新的 Chrome 实例，因此它可能不是性能最好的。这是使用 Chrome 自动化测试的最精确方法，因为它使用的是真正的浏览器。

确切地说，它使用 Chrome 的开源部分 Chromium，这主要意味着它没有 Google 许可且无法开源的专有编解码器（MP3、AAC、H.264..），也没有与崩溃报告、谷歌更新等谷歌服务的集成，但从编程的角度来看，它应该与 Chrome 100% 相似（除了媒体播放）。

## 安装

```bash
npm i puppeteer
```

这将下载并捆绑最新版本的 Chromium。

您可以选择让 puppeteer 运行您已经安装的 Chrome 的本地安装，取而代之的是安装 `puppeteer-core`，这在一些特殊情况下是有用的（参见 [puppeteer 与 puppeteer-core 的区别](https://github.com/puppeteer/puppeteer#puppeteer-core)）。但在通常下，您只需使用 `puppeteer`。

## 基本用法

导入它：

```js
const puppeteer = require('puppeteer')
```

然后，我们可以使用 `launch()` 方法创建一个浏览器实例：

```js
;(async () => {
  const browser = await puppeteer.launch()
})()
```

我们也可以这样写：

```js
puppeteer.launch().then(async (browser) => {
  //...
})
```

您可以将带有选项的对象传递给 `puppeteer.launch()`。最常见的是：

```js
puppeteer.launch({ headless: false })
```

`headless` 表示在 Puppeteer 执行操作时显示 Chrome。这样可以看到正在发生的事情并进行调试。

我们使用 `await`，因此我们必须将此方法调用包装在一个异步函数中，然后立即调用该函数。

接下来，我们可以在 `browser` 对象上使用 `newPage()` 方法来获取 `page` 对象：

```js
;(async () => {
  const browser = await puppeteer.launch()
  const page = await browser.newPage()
})()
```

接下来，我们调用 `page` 对象上的 `goto()` 方法来加载页面：

```js
;(async () => {
  const browser = await puppeteer.launch()
  const page = await browser.newPage()
  await page.goto('https://github.com/lio-zero')
})()
```

我们也可以使用 Promise，而不是 `async/await`，但使用后者会使代码更具可读性：

```js
;(() => {
  puppeteer.launch().then((browser) => {
    browser.newPage().then((page) => {
      page.goto('https://github.com/lio-zero').then(() => {
        //...
      })
    })
  })
})()
```

## 获取页面内容

一旦我们有了一个加载了 URL 的页面，我们可以调用 `page` 的 `evaluate()` 方法来获取页面内容：

```js
;(async () => {
  const browser = await puppeteer.launch()
  const page = await browser.newPage()
  await page.goto('https://github.com/lio-zero')

  const result = await page.evaluate(() => {
    //...
  })
})()
```

该方法采用回调函数，我们可以在其中添加检索所需页面元素所需的代码。我们返回一个新对象，这将是 `evaluate()` 方法调用的结果。

我们可以使用 `page.$()` 方法访问文档上的 Selectors API 方法 `querySelector()`，以及 `page.$$()`作为`querySelectorAll()` 的别名。

计算完成后，我们在浏览器上调用 `browser.close()` 方法：

```js
browser.close()
```

## Page 方法

我们在上面看到了调用 `browser.newPage()` 得到的 `page` 对象，我们用它调用了 `goto()` 和 `evaluate()` 方法。

所有方法都返回一个 Promise，因此它们通常带有 `await` 关键字。

让我们看看我们将调用的一些最常用的方法。您可以在 Puppeteer 文档上看到[完整的列表](https://pptr.dev/#?product=Puppeteer&show=api-class-page)。

### page.$()

允许访问页面上的 `querySelector()` 方法

### page.$$()

允许访问页面上的 `querySelectorAll()` 方法

### page.$eval()

接受 2 个或更多参数。第一个是选择器，第二个是函数。如果有更多参数，则这些参数将作为附加参数传递给函数。

它在页面上运行 `querySelectorAll()`，使用第一个参数作为选择器，然后将该参数用作函数的第一个参数。

```js
const innerTextOfButton = await page.$eval(
  'button#submit',
  (el) => el.innerText
)
```

### click()

在作为参数传递的元素上执行鼠标单击事件：

```js
await page.click('button#submit')
```

我们可以传递一个带有选项对象的附加参数：

- `button` 可以设置为 `left`（默认）、`right` 或 `middle`
- `clickCount` 是一个默认为 1 的数字，用于设置应单击元素的次数
- `delay` 是点击之间的毫秒数。默认为 `0`

### content()

获取页面的 HTML 源代码：

```js
const source = await page.content()
```

### emulate()

`emulate()` 用于模拟设备。它将用户代理设置为特定设备，并相应地设置视口。

[此文件](https://github.com/GoogleChrome/puppeteer/blob/master/DeviceDescriptors.js)中提供了支持的设备列表。

以下是模拟 iPhone X 的方法：

```js
const puppeteer = require('puppeteer')
const device = require('puppeteer/DeviceDescriptors')['iPhone X']

puppeteer.launch().then(async (browser) => {
  const page = await browser.newPage()
  await page.emulate(device)

  //do stuff

  await browser.close()
})
```

### evaluate()

在页面上下文中计算函数。在这个函数中，我们可以访问 `document` 对象，因此我们可以调用任何 DOM API：

```js
const puppeteer = require('puppeteer')

;(async () => {
  const browser = await puppeteer.launch()
  const page = await browser.newPage()
  await page.goto('https://github.com/lio-zero')

  const result = await page.evaluate(() => {
    return document.querySelector('.avatar-user').length
  })

  console.log(result)
})()
```

我们在此处调用的任何内容都在页面上下文中执行，因此如果我们运行 `console.log()`，我们将不会在 Node.js 上下文中看到结果，因为这是在无头浏览器中执行的。

我们可以在这里计算值并返回 JavaScript 对象，但如果我们想返回一个 DOM 元素并在 Node.js 上下文中访问它，我们必须使用不同的方法 `evaluateHandle()`。如果我们从 `evaluate()` 返回一个 DOM 元素，我们只会得到一个空对象。

### evaluateHandle()

类似于 `evaluate()`，但是如果我们返回一个 DOM 元素，我们将得到正确的对象而不是一个空对象：

```js
const puppeteer = require('puppeteer')

;(async () => {
  const browser = await puppeteer.launch()
  const page = await browser.newPage()
  await page.goto('https://github.com/lio-zero')

  const result = await page.evaluateHandle(() => {
    return document.querySelector('.avatar-user')
  })

  console.log(result)
})()
```

### exposeFunction()

此方法允许您在浏览器上下文中添加一个新函数，该函数在 Node.js 上下文中执行。

这意味着我们可以添加一个在浏览器中运行 Node.js 代码的函数。

此示例在浏览器上下文中添加一个 `test()` 函数，该函数从文件系统读取 `app.js` 文件，其路径相对于脚本：

```js
const puppeteer = require('puppeteer')
const fs = require('fs')

;(async () => {
  const browser = await puppeteer.launch()
  const page = await browser.newPage()
  await page.goto('https://github.com/lio-zero')

  await page.exposeFunction('test', () => {
    const loadData = (path) => {
      try {
        return fs.readFileSync(path, 'utf8')
      } catch (err) {
        console.error(err)
        return false
      }
    }
    return loadData('app.js')
  })

  const result = await page.evaluate(() => {
    return test()
  })

  console.log(result)
})()
```

### focus()

聚焦元素：

```js
await page.focus('input#name')
```

### goBack()

返回页面导航历史：

```js
await page.goBack()
```

### goForward()

在页面导航历史中前进：

```js
await page.goForward()
```

### goto()

打开一个新页面：

```js
await page.goto('https://github.com/lio-zero')
```

您可以使用选项将对象作为第二个参数传递。其中，如果传递了 `waitUntil: networkidle2`，将等到导航完成：

```js
await page.goto('https://github.com/lio-zero', { waitUntil: 'networkidle2' })
```

### hover()

在作为参数传递的选择器上执行鼠标悬停：

```js
await page.hover('input#name')
```

### pdf()

从页面生成 PDF：

```js
await page.pdf({ path: 'file.pdf' })
```

您可以将许多[选项](https://pptr.dev/api/puppeteer.pdfoptions)传递给此方法，以设置生成的 PDF 详细信息。

> 注意：`launch` 方法不要设置 `headless` 选项的值为 `false`。

### reload()

重新加载页面：

```js
await page.reload()
```

### screenshot()

获取页面的 PNG 截图，将其保存到使用 `path` 选择的文件名：

```js
await page.screenshot({ path: 'screenshot.png' })
```

[查看所有选项](https://pptr.dev/api/puppeteer.screenshotoptions)

### select()

选择作为参数传递的选择器标识的 DOM 元素

```js
await page.select('input#name')
setContent()
```

您可以设置页面的内容，而不是打开现有的网页。

对于以编程方式使用现有 HTML 生成 PDF 或屏幕截图非常有用：

```js
const html = '<h1>Hello!</h1>'
await page.setContent(html)
await page.pdf({ path: 'hello.pdf' })
await page.screenshot({ path: 'screenshot.png' })
```

### setViewPort()

默认情况下，视口为 `800 x 600`。如果您想有一个不同的视口，也许需要截屏，调用 `setViewport` 传递一个带有 `width` 和 `height` 属性的对象。

```js
await page.setViewport({ width: 1280, height: 800 })
```

### title()

获取页面标题：

```js
await page.title()
```

### type()

输入标识表单元素的选择器

```js
await page.type('input#name', 'lotto')
```

`delay` 选项允许像真实用户一样模拟打字，在每个字符之间添加延迟：

```js
await page.type('input#name', 'lotto', { delay: 100 })
```

### url()

获取页面地址：

```js
await page.url()
```

### viewport()

获取页面视口：

```js
await page.viewport()
```

### waitFor()

等待特定的事情发生。具有以下快捷功能：

- `waitForFunction`
- `waitForNavigation`
- `waitForRequest`
- `waitForResponse`
- `waitForSelector`
- `waitForXPath`

例子：

```js
await page.waitFor(waitForNameToBeFilled)
const waitForNameToBeFilled = () => page.$('input#name').value != ''
```

## Page 命名空间

`page` 对象使您可以访问几个不同的对象：

- `accessibility`
- `coverage`
- `keyboard`
- `mouse`
- `touchscreen`
- `tracing`

每一个都有很多新功能。

`keyboard` 何 `mouse` 很可能是您在尝试使事情自动化时最常使用的那些。

例如，这是您触发 `input` 元素的方式（之前应该已选择）：

```js
await page.keyboard.type('hello!')
```

其他键盘方法是：

- `keyboard.down()` 发送 `keydown` 事件
- `keyboard.press()` 发送一个 `keydown` 后跟一个 `keyup`（模拟普通键类型）。主要用于修饰键（`shift`、`ctrl` 和 `cmd`）
- `keyboard.sendCharacter()` 发送按键事件
- `keyboard.type()` 发送 `keydown`、`keypress` 和 `keyup` 事件
- `keyboard.up()` 发送 `keyup` 事件

`mouse` 提供 4 种方法：

- `mouse.click()` 模拟点击：`mousedown` 和 `mouseup` 事件
- `mouse.down()` 模拟 `mousedown` 事件
- `mouse.move()` 移动到不同的坐标
- `mouse.up()` 模拟 `mouseup` 事件

## 更多资料

- [FAQ](https://pptr.dev/faq) 和 [Troubleshooting](https://pptr.dev/troubleshooting/) — puppeteer 常见问题

对于 Puppeteer，还有许多其他用例和工具可能需要更多的曝光：

- Puppeteer 提供了多个的[用例](https://github.com/puppeteer/examples)。例如，[自动视觉扩散](https://meowni.ca/posts/2017-puppeteer-tests/)或[监视每个构建中未使用的 CSS](http://blog.cowchimp.com/monitoring-unused-css-by-unleashing-the-devtools-protocol/)
- [Web Performance Recipes With Puppeteer](https://addyosmani.com/blog/puppeteer-recipes/)
- [记录和生成 Puppeteer 和 Playwright scripts 的有用工具](https://twitter.com/addyosmani/status/1219884904307142659)
- 此外，您甚至可以在 [DevTools 中记录测试](https://twitter.com/umaar/status/1331924138970255360)
- Nitay Neeman 对 [Puppeteer 的全面概述](https://nitayneeman.com/posts/getting-to-know-puppeteer-using-practical-examples/)，包括示例和用例。
- [Puppeteer & Playwright 学习教程](https://www.checklyhq.com/learn/headless/)，介绍无头浏览器操作库 Puppeteer 和 Playwright 的用法。
- [Puppeteer 资源社区列表](https://github.com/transitive-bullshit/awesome-puppeteer)
- [Headless Chrome vs PhantomJS Benchmark](https://hackernoon.com/benchmark-headless-chrome-vs-phantomjs-e7f44c6956c)
