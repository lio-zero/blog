# Web Notification API

[Notifications API](https://developer.mozilla.org/en-US/docs/Web/API/notification) 是浏览器向开发人员公开的接口，允许在用户允许的情况下向用户显示消息，即使网站/Web 应用未在浏览器中打开。

这些消息是一致的和原生的，这意味着接收者习惯于它们的 UI 和 UX（用户界面和用户体验），是系统范围的，而不是特定于您的网站。

与 Push API 相结合，这项技术可以成为提高用户参与度和增强应用功能的成功途径。

> Notifications API 与 Service Worker 进行了大量的交互，因为它是推送通知所需要的。您可以使用 Notifications API 而不使用 Push API，但是它的用例是有限的。

```js
if (window.Notification && Notification.permission !== 'denied') {
  Notification.requestPermission((status) => {
    // 如果用户接受，状态为 granted
    const n = new Notification('Title', {
      body: '我是正文内容!',
      icon: '/path/to/icon.png' // 可选
    })
  })
}

n.close()
```

## 权限

要向用户显示通知，您必须具有这样做的权限。

`Notification.requestPermission()` 方法调用请求此权限。

你可以调用：

```js
Notification.requestPermission()
```

它将请求弹出一个选项框：

![授予权限](https://upload-images.jianshu.io/upload_images/18281896-279e258846894ecd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

它将显示一个权限授予面板，除非之前已经授予了权限。

要在用户交互（允许或拒绝）时执行操作，您可以向其附加处理函数：

```js
const process = (permission) => {
  if (permission === 'granted') {
    // do something
  }
}

Notification.requestPermission((permission) => {
  process(permission)
}).then((permission) => {
  process(permission)
})
```

看看我们是如何传入回调并期待一个 promise 的。这是因为过去对 `Notification.requestPermission()` 的不同实现，我们现在必须支持它，因为我们事先不知道浏览器中运行的是哪个版本。因此，为了将内容保存在一个位置，我在 `process()` 函数中提取了权限处理。

在这两种情况下，函数都会被传递一个 `permission` 字符串，该字符串可以具有以下值之一：

- `granted` — 用户接受，我们可以显示权限
- `denied` — 用户拒绝，我们无法显示任何权限
- 也可以通过检查 `Notification.permission` 属性来检索这些值，如果用户已经授予权限，则该属性的计算结果为 `granted` 或 `denied`，但如果您尚未调用 `Notification.requestPermission()`，它将解析为 `default`。

## 创建通知

浏览器中 `window` 对象公开的 `Notification` 对象，允许您创建通知并自定义其外观。

下面是一个最简单的示例，在您请求权限后生效：

```js
Notification.requestPermission()
new Notification('Hey')
```

![创建通知](https://upload-images.jianshu.io/upload_images/18281896-3216545d1ddc3ef7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

您有几个选项可以自定义通知。

## 添加正文

首先，可以添加一个正文，正文通常显示为单行：

```js
new Notification('Hey', {
  body: '您应该关注 lio!'
})
```

![向通知添加正文](https://upload-images.jianshu.io/upload_images/18281896-2930ba459622cc7d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 添加图像

您可以添加 `icon` 属性：

```js
new Notification('Hey', {
  body: '您应该关注 lio!',
  icon: 'https://github.com/fluidicon.png'
})
```

![在通知中添加图像](https://upload-images.jianshu.io/upload_images/18281896-9b8b9881e43adee4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 关闭通知

您可能希望在打开通知后将其关闭。

为此，请创建对您打开的通知的引用：

```js
const n = new Notification('Hey')
```

然后，您可以稍后使用以下命令将其关闭：

```js
n.close()
```

或超时：

```js
setTimeout(n.close(), 1000)
```

## 更多资料

- [Notifications API](https://notifications.spec.whatwg.org/)
- [An Introduction to the Web Notifications API](https://www.sitepoint.com/introduction-web-notifications-api/)
