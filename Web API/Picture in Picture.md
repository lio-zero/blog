# Picture in Picture

[Picture-in-picture](https://developer.mozilla.org/en-US/docs/Web/API/Picture-in-Picture_API) （画中画）功能允许用户在一个小的叠加窗口中弹出网页中播放的视频，用于在浮动窗口上显示内容。它允许用户在与背景页面和其他网站交互时继续查看内容。

[Picture in Picture 支持情况](https://caniuse.com/picture-in-picture)：

![Picture in Picture 支持情况](https://upload-images.jianshu.io/upload_images/18281896-d69ddefd4562fb13.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 实现画中画

一个 `video` 和一个切换画中画功能的按钮。

```html
<video id="video" src="video.mp4" controls autoplay />
<button id="toggle">toggle</button>
```

JS 只需要调用 `video` 元素的 `requestPictureInPicture` 方法即可，然后再调用 `document` 对象下的 `exitPictureInPicture` 方法就可以关闭画中画功能了。

另外，`document` 对象下的 `pictureInPictureElement` 指向画中画功能生效的那个 `video` 元素，如果没有开启画中画，那么返回值是 `null`

`requestPictureInPicture` 和 `exitPictureInPicture` 都是异步 API 的，返回 Promise

```js
// 进入画中画
video.requestPictureInPicture()
// 退出画中画
document.exitPictureInPicture()
// 画中画生效的 video 元素
document.pictureInPictureElement
```

我们可以检查 API 是否受支持，如下所示：

```js
const hasSupport = () => Boolean('pictureInPictureEnabled' in document)
```

## 画中画活动

浏览器使我们能够检测视频何时进入画中画模式或离开画中画模式。由于可以进入或退出画中画模式，因此最好依靠事件检测来更新媒体控件。

事件分别为 `enterpictureinpicture` 和 `leavepictureinpicture`，它们在视频进入或退出画中画模式时触发。

在我们的示例中，我们需要根据视频当前是否处于画中画模式来更新 `button` 标签。

```js
video.addEventListener('enterpictureinpicture', () => {
  button.textContent = '退出画中画模式'
})

video.addEventListener('leavepictureinpicture', () => {
  button.textContent = '进入画中画模式'
})
```

## 自定义画中画窗口

默认情况下，浏览器在画中画窗口中显示**播放/暂停**按钮，除非视频正在播放[MediaStream 对象](https://developer.mozilla.org/en-US/docs/Web/API/MediaStream)（由虚拟视频源（如相机，视频记录设备，屏幕共享服务或其他硬件产生）来源）。

还可以添加直接从画中画窗口转到上一曲目或下一曲目的控件：

```js
navigator.mediaSession.setActionHandler('previoustrack', () => {
  // Go to previous track
})

navigator.mediaSession.setActionHandler('nexttrack', () => {
  // Go to next track
})
```

## 禁用视频中的画中画

`video` 的 `disablePictureInPicture` 属性可以禁用画中画：

```html
<video disablePictureInPicture controls src="video.mp4" />
```

为了减少代码占用太篇幅，没有提供完整的示例，点击此处 👉 查看[完整示例](https://codepen.io/lio-zero/pen/yLKyrwZ)。

## 进一步阅读

- [Picture-in-Picture standard](https://wicg.github.io/picture-in-picture)
- [Picture-in-Picture Sample](https://googlechrome.github.io/samples/picture-in-picture/)
- [An Introduction to the Picture-in-Picture Web API](https://css-tricks.com/an-introduction-to-the-picture-in-picture-web-api/)
