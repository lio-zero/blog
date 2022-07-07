# JavaScript BOM 详解

## 什么是 BOM？

BOM（Browser Object Model）浏览器对象模型，允许 JavaScript 与浏览器 "对话"，是对浏览器本身进行操作的。

BOM 不是标准化的，最初是 Netscape 浏览器标准的一部分，它可以根据不同的浏览器进行更改。

## Window

所有浏览器都支持 window 对象。它表示浏览器窗口。所有 JavaScript 全局对象、函数以及变量均自动成为 window 对象的成员。

全局变量是 window 对象的属性。全局函数是 window 对象的方法。它提供与浏览器的交互 5 大属性，都是 `window` 对象的属性

- `document` 文档对象；
- `location` 浏览器当前 URL 信息；
- `navigator` 浏览器本身信息；
- `screen` 客户端屏幕信息；
- `history` 浏览器访问历史信息；

`document` 对象我们留到 DOM 篇再说。下面将介绍其他四个对象和一些常用的方法。详细内容可看 [MDN](https://developer.mozilla.org/zh-CN/docs/Web/API)。本文的一些例子来自 [30 seconds of code](https://www.30secondsofcode.org/)

使用时，可以不带 `window` 调用它们。如：`window.document.body` 可以直接写出 `document.body`。

**Window 对象常见的属性**

```js
closed	      // 返回窗口是否已被关闭。
defaultStatus // 设置或返回窗口状态栏中的默认文本。
document	  // 对 Document 对象的只读引用。请参阅 Document 对象。
history		  // 对 History 对象的只读引用。请参数 History 对象。
innerheight	  // 返回窗口的文档显示区的高度。
innerwidth	  // 返回窗口的文档显示区的宽度。
length		  // 设置或返回窗口中的框架数量。
location	  // 用于窗口或框架的 Location 对象。请参阅 Location 对象。
name		  // 设置或返回窗口的名称。
Navigator	  // 对 Navigator 对象的只读引用。请参数 Navigator 对象。
opener		  // 返回对创建此窗口的窗口的引用。
outerheight	  // 返回窗口的外部高度。
outerwidth	  // 返回窗口的外部宽度。
pageXOffset	  // 设置或返回当前页面相对于窗口显示区左上角的 X 位置。
pageYOffset	  // 设置或返回当前页面相对于窗口显示区左上角的 Y 位置。
parent		  // 返回父窗口。
Screen		  // 对 Screen 对象的只读引用。请参数 Screen 对象。
self		  // 返回对当前窗口的引用。等价于 Window 属性。
status		  // 设置窗口状态栏的文本。
top			  // 返回最顶层的先辈窗口。
window		  // window 属性等价于 self 属性，它包含了对窗口自身的引用。
screenLeft、screenTop、screenX、screenY // 只读整数。声明了窗口的左上角在屏幕上的的 x 坐标和 y 坐标。IE、Safari 和 Opera 支持 screenLeft 和 screenTop，而 Firefox 和 Safari 支持 screenX 和 screenY。

console       // 只读。返回 console 对象的引用，该对象提供了对浏览器调试控制台的访问。
frames        // 只读。返回当前窗口中所有子窗体的数组。
history		  // 只读。返回一个对 history 对象的引用。
indexedDB	  // 只读。提供应用程序异步访问索引数据库功能的机制；返回 IDBFactory 对象。
localStorage  // 只读。返回用来存储只能在创建它的源下访问的数据的本地存储对象的引用
sessionStorage // 返回对用于存储数据的 session 存储对象的引用，这些数据只能由创建它的源访问。
```

**Window 对象常见的方法**

```js
alert() // 显示带有一段消息和一个确认按钮的警告框。
blur() // 把键盘焦点从顶层窗口移开。
clearInterval() // 取消由 setInterval() 设置的 timeout。
clearTimeout() // 取消由 setTimeout() 方法设置的 timeout。
close() // 关闭浏览器窗口。
confirm() // 显示带有一段消息以及确认按钮和取消按钮的对话框。
createPopup() // 创建一个 pop-up 窗口。
focus() // 把键盘焦点给予一个窗口。
moveBy() // 可相对窗口的当前坐标把它移动指定的像素。
moveTo() // 把窗口的左上角移动到一个指定的坐标。
open() // 打开一个新的浏览器窗口或查找一个已命名的窗口。
print() // 打印当前窗口的内容。
prompt() // 显示可提示用户输入的对话框。
resizeBy() // 按照指定的像素调整窗口的大小。
resizeTo() // 把窗口的大小调整到指定的宽度和高度。
scrollBy() // 按照指定的像素值来滚动内容。
scrollTo() // 把内容滚动到指定的坐标。
setInterval() // 按照指定的周期（以毫秒计）来调用函数或计算表达式。
setTimeout() // 在指定的毫秒数后调用函数或计算表达式。

scroll() // 滚动窗口到文档中的特定位置。
requestAnimationFrame() // 告诉浏览器一个动画正在进行中，请求浏览器为下一个动画帧重新绘制窗口。
postMessage() // 为一个窗口向另一个窗口发送数据字符串提供了一种安全方法，该窗口不必与第一个窗口处于相同的域中。
getSelection() // 返回一个 Selection 对象，表示用户选择的文本范围或光标的当前位置。
getComputedStyle() // 获取指定元素的计算样式。Computed 样式指示元素的所有 CSS 属性的计算值。
```

### 返回当前页面的滚动位置

- 使用 `Window.pageXOffset` 和（`Window.pageYOffset` 如果已定义），否则使用 `Element.scrollLeft` 和 `Element.scrollTop`。
- 忽略单个参数 `el` 使用默认值 `window`。

```js
const getScrollPosition = (el = window) => ({
  x: el.pageXOffset !== undefined ? el.pageXOffset : el.scrollLeft,
  y: el.pageYOffset !== undefined ? el.pageYOffset : el.scrollTop
})
getScrollPosition() // {x: 0, y: 200}
```

### 检查是否支持触摸事件

检查 `window` 中是否存在 `'ontouchstart'`

```js
const supportsTouchEvents = () => window && 'ontouchstart' in window

supportsTouchEvents() // true
```

### 获取当前选定的文本

使用 `Window.getSelection()` 和 `Selection.toString()` 获取当前选择的文本。

```js
const getSelectedText = () => window.getSelection().toString()

btn.addEventListener('click', function () {
  console.log(getSelectedText()) // 'Lorem ipsum'
})
```

### 平滑滚动至页面顶部

```js
const scrollToTop = () => {
  const c = document.documentElement.scrollTop || document.body.scrollTop
  if (c > 0) {
    window.requestAnimationFrame(scrollToTop)
    window.scrollTo(0, c - c / 8)
  }
}

scrollToTop()
```

### 滚动到页面顶部

```js
const goToTop = () => window.scrollTo(0, 0)
goToTop()
```

## Location

`window.location` 对象用于获得当前页面的地址 (URL)，并把浏览器重定向到新的页面。

```js
location.hash // 设置或返回从井号 (#) 开始的 URL（锚）。
location.host // 设置或返回主机名和当前 URL 的端口号。
location.href // 设置或返回完整的 URL。
location.port // 返回 web 主机的端口 （80 或 443）
location.search // 设置或返回从问号 (?) 开始的 URL（查询部分）。
location.hostname // 返回 web 主机的域名
location.pathname // 返回当前页面的路径和文件名
location.protocol // 返回所使用的 web 协议（http: 或 https:）

location.reload() // 重新加载当前文档。
```

`location.replace(url)` 方法以给定的 URL 来替换当前的资源。

```js
// 将跳转到该链接相应的页面
location.replace('https://github.com/lio-zero')
```

`location.assign(url)` 方法会触发窗口加载并显示指定的 URL 的内容。加载新的文档。

```js
// 跳转到给定 URL 的页面
location.assign('https://github.com/lio-zero')
```

`replace()` 和 `assign()` 不同在于，调用 `replace()` 方法后，当前页面不会保存到会话历史中，这样，用户点击回退按钮时，将不会再跳转到该页面。（不保存跳转前的页面）

### 返回上一页

```js
history.go(-1) // 返回上一页
history.back() // 返回上一页

// 强行刷新（返回上一页刷新页面）：
history.back()
location.reload()

location.go(-1) // 刷新上一页
```

### 返回当前 URL

使用 `Window.location.href` 来获得当前的 URL。

```js
const currentURL = () => window.location.href
currentURL() // 'https://www.baidu.com/'
```

### `HTTP` 跳转 `HTTPS`

如果页面当前在 HTTP 中，则将其重定向到 HTTPS

```js
const httpsRedirect = () => {
  if (location.protocol !== 'https:')
    location.replace('https://' + location.href.split('//')[1])
}

httpsRedirect() // 若在 http://www.baidu.com, 则跳转到 https://www.baidu.com
```

### 重定向到指定的 URL

- 使用 `Window.location.href` 或 `Window.location.replace()` 重定向到 `url`。
- 传递第二个参数来模拟链接单击（`true`-默认）或 HTTP 重定向（`false`）。

```js
const redirect = (url, asLink = true) =>
  asLink ? (window.location.href = url) : window.location.replace(url)

redirect('https://juejin.cn/user/96412754251390')
```

### 获取当前页面上正在使用的协议

使用 `window.location.protocol` 来获取协议（`http:` 或 `https:` 当前页面）。

```js
const getProtocol = () => window.location.protocol

getProtocol() // 'https:'
```

## Navigator

`window.navigator` 对象包含有关访问者浏览器的信息。

```js
navigator.appCodeName // 只读。返回所使用浏览器的内部名称。
navigator.appName // 只读。返回所使用浏览器的名称。由于兼容性问题，HTML5 规范允许该属性返回 "Netscape" 。
navigator.appVersion // 只读。该属性已从 Web 标准中删除。但一些浏览器还在使用。它返回一个字符串，表示所使用浏览器的版本号。
navigator.cookieEnabled // 只读。返回一个布尔值，来表示当前页面是否启用了 cookie。
navigator.platform // 只读。返回一个字符串,表示浏览器所在的系统平台类型.
navigator.userAgent // 只读。返回当前浏览器的 user agent（用户代理）字符串。
navigator.product // 只读。返回当前浏览器的产品名称。在任意浏览器下都只返回 'Gecko'，此属性仅用于兼容的目的。
navigator.onLine // 只读。返回浏览器的联网状态。正常联网（在线）返回 true，不正常联网（离线）返回 false。
navigator.language // 只读。返回一个表示用户偏好语言的字符串，通常指浏览器 UI 的语言。
navigator.languages // 只读。返回一个数组，数组内容表示网站访客所使用的语言。使用 BCP 47 语言标签来描述不同的语言。在返回的数组中，最适合当前用户的语言将会被排到数组的首位。它占时只在一些浏览器内被使用。
navigator.javaEnabled() // 只读。返回 Boolean 值，表明浏览器是否支持 Java。
```

**注意**：来自 `navigator` 对象的信息具有误导性，不应该被用于检测浏览器版本，这是因为：

- `navigator` 数据可被浏览器使用者更改
- 一些浏览器对测试站点会识别错误
- 浏览器无法报告晚于浏览器发布的新操作系统

### 浏览器检测

由于 navigator 可误导浏览器检测，使用对象检测可用来嗅探不同的浏览器。

由于不同的浏览器支持不同的对象，您可以使用对象来检测浏览器。例如，由于只有 Opera 支持属性 `window.opera`，您可以据此识别出 Opera。

```js
if (window.opera) { ... }
```

### 如何检测浏览器版本（UA 检测）？

```js
// 如何检测是否为 Chrome 浏览器
var ua = navigator.userAgent
console.log(ua)
var isChrome = ua.includes('Chrome')
console.log(isChrome) // true 为是，false 为不是
```

### 检测页面是在移动/PC 设备上查看

使用正则表达式测试该 `navigator.userAgent` 属性，以确定该设备是移动设备还是台式机。

```js
const detectDeviceType = () =>
  /Android|webOS|iPhone|iPad|iPod|BlackBerry|IEMobile|Opera Mini/i.test(
    navigator.userAgent
  )
    ? 'Mobile'
    : 'Desktop'
detectDeviceType() // 'Mobile' or 'Desktop'
```

你可以切换浏览器进行测试。

### 检查当前用户是否为苹果设备

```js
const isAppleDevice = /Mac|iPod|iPhone|iPad/.test(navigator.platform)
console.log(isAppleDevice)
```

### 检测当前用户的首选语言

- 使用 `navigation.language` 或 `navigation.languages` 的第一个，否则返回 `defaultLang`。
- 省略参数 `defaultLang`，将以 `'zh-CN'` 用作默认语言。

```js
const detectLanguage = (defaultLang = 'zh-CN') =>
  navigator.language ||
  (Array.isArray(navigator.languages) && navigator.languages[0]) ||
  defaultLang

detectLanguage() // 'zh-CN'
```

## Screen

`window.screen` 对象包含有关用户屏幕的信息。

```js
screen.height // 返回当前屏幕的高度。(以像素为单位)
screen.width // 返回当前屏幕的宽度。(以像素为单位)
screen.availWidth // 可用的屏幕宽度
screen.availHeight // 可用的屏幕高度
screen.colorDepth // 色彩深度
screen.pixelDepth // 色彩分辨率
screen.colorDepth // 返回目标设备或缓冲器上的调色板的比特深度。
```

## History

`window.history` 对象包含浏览器的历史。

```js
history.back() // 方法加载历史列表中的前一个 URL，与在浏览器点击后退按钮相同
history.forward() // 方法加载历史列表中的下一个 URL，与在浏览器中点击向前按钮相同

history.go(number | URL) // 方法可加载历史列表中的某个具体的页面。
history.go(0) // 参数为 0，表示刷新页面
history.go(1) // 参数为 1，表示前进一个页面
history.go(-1) // 参数为 -1，表示后退一个页面
```

HTML5 新增了两个方法：`pushState()` 和 `replaceState()`

**history.pushState 添加浏览器记录**

`history.pushState(state, title[, url])` 方法向当前浏览器会话的历史堆栈中添加一个状态（state）。

- `state` 状态对象是一个 JavaScript 对象，它与 `pushState()` 创建的新历史记录条目相关联。每当用户导航到新状态时，都会触发 `popstate` 事件，并且该事件的状态属性包含历史记录条目的状态对象的副本。
- `title` [当前大多数浏览器都忽略此参数](https://github.com/whatwg/html/issues/2174)，尽管将来可能会使用它。 在此处传递空字符串应该可以防止将来对方法的更改。 或者，您可以为该状态传递简短标题。
- `url` 新历史记录条目的 URL 由此参数指定。

以下代码通过设置 `state`，`title` 和 `url` 创建一条新的历史记录。

```js
const state = { page_id: 1, user_id: 5 }
const title = ''
const url = 'home.html'

history.pushState(state, title, url)
```

上面代码中，将在浏览器历史记录插入一条新的记录。**注意**：当前页面的搜索栏会发生变化，但是不会刷新当前页面。

**history.replaceState 修改浏览器记录**

`history.replaceState(stateObj, title[, url])` 方法用来修改当前历史记录实体，如果你想更新当前的 `stateObj` 或者当前历史实体的 `URL` 来响应用户的的动作的话这个方法将会非常有用。

- `stateObj` 状态对象是一个 JavaScript 对象，它与传递给 `replaceState` 方法的历史记录实体相关联。
- `title` [大部分浏览器忽略这个参数](https://github.com/whatwg/html/issues/2174)，将来可能有用。在此处传递空字符串应该可以防止将来对方法的更改。或者，您可以为该状态传递简短标题
- `url` 历史记录实体的 URL。新的 URL 跟当前的 URL 必须是同源；否则 `replaceState` 抛出一个异常。

```js
const stateObj = { foo: 'bar' }
history.pushState(stateObj, '', 'bar.html')
```

**popState 事件在用户返回或前进进会被出发触发**

> 需要注意的是调用 `history.pushState()` 或 `history.replaceState()` 不会触发 `popstate` 事件。只有在做出浏览器动作时，才会触发该事件，如用户点击浏览器的回退按钮（或者在 JavaScript 代码中调用 `history.go()`、`history.back()` 或者`history.forward()`方法）
>
> 不同的浏览器在加载页面时处理 `popstate` 事件的形式存在差异。页面加载时 Chrome 和 Safari 通常会触发(emit )`popstate` 事件，但 Firefox 则不会。

```js
window.addEventListener('popstate', (e) => {
  console.log(e.state) // pushState 或 replaceState 的第一个参数
})
```

## 检查是否为浏览器环境

```js
const isBrowser = () => ![typeof window, typeof document].includes('undefined')

isBrowser() // true (browser)
isBrowser() // false (Node)
```

## 生成一个包含当前 URL 参数的对象

```js
// 方法一：
const getURLParameters = (url) =>
  (url.match(/([^?=&]+)(=([^&]*))/g) || []).reduce(
    (a, v) => (
      (a[v.slice(0, v.indexOf('='))] = v.slice(v.indexOf('=') + 1)), a
    ),
    {}
  )

getURLParameters('google.com') // {}
getURLParameters('https://url.com?name=lisi&age=18') // {name: 'lisi', age: '18'}

// 方法二：
const queryStringToObject = (url) =>
  [...new URLSearchParams(url.split('?')[1])].reduce(
    (a, [k, v]) => ((a[k] = v), a),
    {}
  )

queryStringToObject('https://google.com?page=1&count=10') // {page: "1", count: "10"}
```
