# getUserMedia() 方法

由 `navigator.mediaDevices` 暴露 MediaDevices 的对象为我们提供了 `getUserMedia` 方法。

> **注意**：`navigator` 对象还公开了一个 `getUserMedia()` 方法，该方法可能仍然有效，但已弃用。为了保持一致性，API 已移动到 `mediaDevices` 对象内部。

假设我们有一个按钮：

```html
<button>开启流媒体</button>
```

我们一直等到用户点击这个按钮，然后调用 `navigator.mediaDevices.getUserMedia()` 方法。

我们传递一个描述所需媒体约束的对象。如果我们需要视频输入，我们会调用：

```js
navigator.mediaDevices.getUserMedia({
  video: true
})
```

我们可以非常具体地处理这些约束：

```js
navigator.mediaDevices.getUserMedia({
  video: {
    minAspectRatio: 1.333,
    minFrameRate: 60,
    width: 640,
    height: 480
  }
})
```

您可以通过调用 `navigator.mediaDevices.getSupportedConstraints()` 获得设备支持的所有约束的列表。

如果我们只想要音频，我们可以通过 `audio: true`：

```js
navigator.mediaDevices.getUserMedia({
  audio: true
})
```

如果我们两者都想要：

```js
navigator.mediaDevices.getUserMedia({
  video: true,
  audio: true
})
```

此方法返回一个 promise，因此我们将使用 `async/await` 在 `stream` 变量中获取结果：

```js
document.querySelector('button').addEventListener('click', async (e) => {
  const stream = await navigator.mediaDevices.getUserMedia({
    video: true
  })
})
```

单击按钮将触发浏览器中的面板，以允许使用媒体设备：

![权限画面](https://upload-images.jianshu.io/upload_images/18281896-ab865f8bae19b5d4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

完成后，我们从 `getUserMedia()` 获得的 `stream` 对象现在可以用于各种事情。最直接的方法是在页面中的 `video` 元素中显示视频流：

```html
<button>开启流媒体</button>

<video autoplay />
```

```js
document.querySelector('button').addEventListener('click', async (e) => {
  const stream = await navigator.mediaDevices.getUserMedia({
    video: true
  })
  document.querySelector('video').srcObject = stream
})
```

## 示例

以下示例中，它要求您访问摄像机并在页面中播放视频。

添加两个按钮，一个访问摄像头以开启视频流，另一个停止视频里，然后添加一个具有 `autoplay` 属性的 `video` 元素。

```html
<button id="start">开启视频流</button>
<button id="stop">停止视频流</button>

<video autoplay />
```

JS 监听按钮的点击，然后调用 `navigator.mediaDevices.getUserMedia()` 请求视频。然后，我们在调用 `stream.getUserMedia()` 的结果上调用 `stream.getVideoTracks()` 来访问摄像机的名称。

流被设置为 `video` 标签的源对象，以便可以进行播放：

```js
const start = document.querySelector('#start')
const stop = document.querySelector('#stop')

let stream = null

stop.addEventListener('click', async (e) => {
  stream && stream.stop()
})

start.addEventListener('click', async (e) => {
  try {
    const mediaStream = await navigator.mediaDevices.getUserMedia({
      audio: false,
      video: true
    })

    stream = mediaStream.getVideoTracks()[0]
    alert(`获取视频: ${stream.label}`)

    document.querySelector('video').srcObject = mediaStream
  } catch (error) {
    alert(`${error.name}`)
    console.error(error)
  }
})
```

`getUserMedia()` 的参数还可以指定视频流的附加要求：

```js
navigator.mediaDevices.getUserMedia(
  {
    video: {
      mandatory: { minAspectRatio: 1.333, maxAspectRatio: 1.334 },
      optional: [{ minFrameRate: 60 }, { maxWidth: 640 }, { maxHeight: 480 }]
    }
  },
  successCallback,
  errorCallback
)
```

要获得音频流，您也需要音频媒体对象，然后调用 `stream.getAudioTracks()` 而不是 `stream.getVideoTracks()`。

我们点击停止按钮，通过调用 `track.stop()` 来停止视频流。

[完整示例](https://codepen.io/lio-zero/pen/NWYpMPv)
