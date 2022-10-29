# Page Visibility API

[Page Visibility API](https://developer.mozilla.org/en-US/docs/Web/API/Page_Visibility_API) 用于确定文档在显示器上是否可见的。

以下是 MDN 所描述的：

> 使用选项卡式浏览，任何给定网页都有可能在后台，因此对用户不可见。页面可见性 API 提供了您可以观察的事件，以便了解文档何时可见或隐藏，以及查看页面当前可见性状态的功能。

在过去，如果需要监听可见性变化，可以使用 `blur` 和 `focus` 事件。

```js
window.addEventListener('focus', () => {
  // do something
})

window.addEventListener('blur', () => {
  // do something
})
```

但这种方式不能准确的监听页面是否可见，因为 `blur` 事件是在页面失去焦点时触发的，所以当用户点击搜索栏、控制台或窗口边框等操作时，它就会被触发。

Page Visibility API 的 `visibilitychange` 事件很好的解决了这种情况。

## `visibilitychange` 事件

当用户导航到一个新的页面、关闭标签页、最小化或关闭浏览器都将将触发 [visibilitychange](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/visibilitychange_event) 事件。

```js
// 页面的可见性状态发生变化
document.addEventListener('visibilitychange', () => {
  // do something
})
```

Page Visibility API 提供了两个属性可以检查页面的浏览器选项卡是否聚焦：

- `document.hidden`
- `document.visibilityState`

### `document.hidden`

`document.hidden` 属性检查页面的浏览器选项卡是否可见或隐藏。

```js
const isBrowserTabFocused = () => !document.hidden

isBrowserTabFocused() // true
```

### `document.visibilityState`

`document.visibilityState` 表示页面所处的状态，当前页面的可见性，有三个取值：

- `visible` — 页面彻底不可见。
- `hidden` — 页面至少一部分可见。
- `prerender` — 页面即将或正在渲染，处于不可见状态。

以下示例在页面可见性变化时，修改页面标题：

```js
document.addEventListener('visibilitychange', () => {
  if (document.visibilityState === 'hidden') {
    document.title = '爱我'
  } else {
    document.title = '恨我'
  }
})
```

## 常见用途

- 动画、视频和音频可以在页面显示时播放，在页面隐藏时暂停
- 线上考试用户切换页面的警告提示
- 停止轮询操作，一些实时获取数据的请求
- etc

以下是一个视频播放/暂停的简单示例：

```js
document.addEventListener('visibilitychange', function () {
  if (document.visibilityState === 'hidden') {
    video.pause()
  }

  if (document.visibilityState === 'visible') {
    video.play()
  }
})
```

## 支持情况

以下是 Can I Use 显示的各浏览器支持情况：

![Page Visibility API](https://upload-images.jianshu.io/upload_images/18281896-cf5e202f34832618.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 最后

页面可见性 API 对于节省资源和提高性能特别有用，它使页面在文档不可见时避免执行不必要的任务。

## 更多资料

更详细的内容可以阅读 MDN 或阮一峰的 [Page Visibility API 教程](https://www.ruanyifeng.com/blog/2018/10/page_visibility_api.html)。
