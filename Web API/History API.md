# History API

[History API](https://developer.mozilla.org/en-US/docs/Web/API/History_API) 允许您与浏览器历史交互，触发浏览器导航方法并更改地址栏内容。

它在与现代单页应用程序结合时特别有用，在这些应用程序上，您永远不会对新页面发出服务器端请求，而是页面始终相同：只是内部内容发生了变化。

在浏览器中运行的现代 JavaScript 应用程序如果不与 History API 交互，无论是显式的还是在框架级别上，对用户来说都将是一个糟糕的体验，因为后退和前进按钮都会中断。

此外，在浏览应用程序时，**视图会发生变化，但地址栏不会**。

另外，重新加载按钮也会中断：由于没有深度链接，重新加载页面将使浏览器显示不同的页面。

History API 是在 HTML5 中引入的，现在所有现代浏览器都支持。

![History API 支持情况](https://upload-images.jianshu.io/upload_images/18281896-b724fdd263bfb643.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

IE 从版本 10 开始支持它，如果您需要支持 IE9 及更早版本，请使用 [history.js](https://github.com/browserstate/history.js/) 库。

## 访问 History API

History API 在 `window` 对象上可用，因此您可以这样调用它：`window.history` 或 `history`，因为 `window` 它是全局对象（可以省略）。

## 浏览历史

`window.history` 对象包含浏览器的历史。

让我们从使用 History API 可以做的最简单的事情开始。

返回上一个页面：

```js
history.back()
```

这将转到会话历史记录中的上一个条目。您可以使用转发到下一页：

```js
history.forward()
```

这就像使用浏览器的后退和前进按钮一样。

`go()` 允许您向后或向前导航多个深度级别。例如：

```js
history.go(0) // 参数为 0，表示刷新页面
history.go(-1) // 相当于 history.back()
history.go(-2) // 相当于调用 2 次 history.back()
history.go(1) // 相当于 history.forward()
history.go(3) // 相当于调用 3 次 history.forward()
```

要知道历史记录中有多少条目，可以调用：

```js
history.length
```

## 向历史记录中添加条目

使用 `pushState()` 可以通过编程方式创建新的历史记录条目。

传递 3 个参数：

- 第一个是可以包含任何内容的对象（但是有大小限制，并且对象需要可序列化）。
- 第二个参数目前未被主要浏览器使用，因此通常传递一个空字符串。
- 第三个参数是与新状态关联的 URL。请注意，该 URL 需要属于当前 URL 的同一个源域。

```js
const state = { name: 'O.O' }

history.pushState(state, '', '/user')
```

上面代码中，将在浏览器历史记录中插入一条新的记录。

调用它不会改变页面的内容，也不会像更改 `window.location` 那样引起任何浏览器操作。

> 当前页面的搜索栏会发生变化，但是不会刷新当前页面。

## 修改历史记录条目

`pushState()` 允许您向历史添加新状态，而 `replaceState()` 允许您编辑当前历史状态。

```js
history.pushState({}, '', '/posts')

const state = { post: 'first' }
history.pushState(state, '', '/post/first')

const state = { post: 'second' }
history.replaceState(state, '', '/post/second')
```

如果你现在调用：

```js
history.back()
```

浏览器直接转到 `/posts`，因为 `/post/first` 被 `/post/second` 替换。

## 访问当前历史记录条目状态

访问属性：

```js
history.state
```

返回当前状态对象（传递给 `pushState` 或 `replaceState` 的第一个参数）。

## onpopstate 事件

每次活动历史状态更改时（用户前进或后退操作），都会在 `window` 上调用 `popstate` 事件，将当前状态作为回调参数：

```js
window.addEventListener('popstate', (e) => {
  console.log(e.state) // pushState 或 replaceState 的第一个参数
})
```

需要注意的是，调用 `history.pushState()` 或 `history.replaceState()` 不会触发 `popstate` 事件。

只有在做出浏览器动作时，才会触发该事件，例如：用户点击浏览器的前进、后退按钮，或者在 JS 代码中调用 `history.go()`、`history.back()` 和 `history.forward()` 方法。

每次调用 `history.back()`、`history.forward()` 或 `history.go()` 时，都会记录新的状态对象（传递给 `pushState` 或 `replaceState` 的第一个参数）。

还有一点需要注意，不同的浏览器在加载页面时处理 `popstate` 事件的形式存在差异。页面加载时 Chrome 和 Safari 通常会触发 `popstate` 事件，但 Firefox 则不会。
