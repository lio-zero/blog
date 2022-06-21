# Web Share API

> [Web Share API](https://developer.mozilla.org/en-US/docs/Web/API/Navigator/share) 允许网站调用主机平台的本机共享功能的一种方法。

平常我们要分享网页的内容到其他应用，需要通过自己实现分享接口。而该 API 正是为了解决这个问题提出的，用于将数据（文本、URL、图像或文件）从 Web 共享到用户选择的应用程序。

该 API 不仅可以改善网页性能，只需要一个按钮，就可以实现多种分享方式，而且不限制分享目标的数量和类型。您平常使用分享的都可以使用到该 API，包括：社交媒体应用、电子邮件、即时消息、以及本地系统安装的、且接受分享的应用，都会出现在系统的分享弹窗，这对手机网页尤其有用

> **注意**：Web Share API 只有通过 HTTPS 提供内容时才能使用。

以下是 [Can I Use](https://caniuse.com/?search=Web%20Share%20API) 给出 Web Share API 的支持情况：

![Web Share API 支持情况](https://upload-images.jianshu.io/upload_images/18281896-a69a6ef27711a675.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

目前，支持程度相对较低，桌面的 Safari 浏览器，手机的安卓 Chrome、Firefox 浏览器和 iOS Safari 浏览器支持该 API。

## 基本用法

```js
const sharePromise = navigator.share(data)
```

`data` 为要分享的数据的对象。至少需要指定以下一个字段。可选配置包括：

- `url` —— 要分享的 URL。
- `text` —— 要分享的文本内容。
- `title` —— 要分享的标题。
- `files` —— 要分享的文件。

在使用时，可以先检查本机系统是否支持该接口。在不支持 Web Share API 的情况下，`navigator.share` 方法为将返回 `undefined`。

```js
if (navigator.share) {
  // 支持
}
```

以下是一个简单的例子，展示了一个基本的共享操作。作为对按钮单击的响应，此 JavaScript 代码共享当前页面的 `URL`。

```js
const text = {
  title: document.title, // 获取当前标题
  url: document.querySelector('link[rel=canonical]')
    ? document.querySelector('link[rel=canonical]').href
    : document.location.href, // 获取当前网站
  text: 'Hello World'
}

shareBtn.addEventListener('click', async () => {
  navigator
    .share(text)
    .then(() => {
      console.log('谢谢分享！')
    })
    .catch((err) => {
      console.error('Error：', err)
    })
})
```

`navigator.share` 方法将会返回一个 `Promise` 对象。调用该方法后，会立即弹出系统的分享弹窗，一旦用户完成分享，这个 `Promise` 对象就会变为 `resolved` 状态。如果指定的共享数据格式不正确， `promise` 将会立即拒绝；如果用户取消了分享，`promise` 也会拒绝。

因为其返回的是 `Promise` 对象，所以也可以使用更简便的 `async/await` 方法

```js
const text = {
  title: 'Web Share API',
  url: 'https://www.jianshu.com/u/3f644e66afa3'
}

shareBtn.addEventListener('click', async () => {
  try {
    await navigator.share(text)
    console.log('已成功分享数据')
  } catch (err) {
    console.error('分享失败：', err.message)
  }
})
```

大致效果如下：

![Chorme 浏览器](https://upload-images.jianshu.io/upload_images/18281896-abac6866455ee79a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![IOS Safari 浏览器](https://upload-images.jianshu.io/upload_images/18281896-ad6e0bc02d7d1698.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 分享文件

上面介绍可选配置时，我们还看到了一个分享文件的 `files` 字段。

在分享文件之前，我们还需要使用 `navigator.canShare()` 方法，判断这个文件是可以否被分享。

> **注意**：因为不是所有文件都允许分享的，目前只有图像，视频，音频和文本文件可以分享。

如果 `navigator.canShare()` 调用成功，将返回一个布尔值 `true`，表示该文件可以被分享。

```js
const text = {
  files: filesArray,
  title: 'Picture',
  text: 'I am Superman'
}

if (navigator.canShare && navigator.canShare({ files: filesArray })) {
  navigator
    .share(text)
    .then(() => console.log('分享成功'))
    .catch((err) => console.log('分享失败', err))
} else {
  console.log('您的系统不支持共享文件。')
}
```
