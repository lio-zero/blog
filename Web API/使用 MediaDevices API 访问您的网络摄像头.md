# 使用 MediaDevices API 访问您的网络摄像头

[MediaDevices API](https://developer.mozilla.org/zh-CN/docs/Web/API/MediaDevices) 接口提供访问连接媒体输入的设备，如照相机和麦克风，以及屏幕共享等。它可以使你取得任何硬件资源的媒体数据。

> **注意**：MediaDevices API 只有通过 HTTPS 提供内容时，才能访问网络摄像头。该 API 还基于 Promise 规范。

以下是 [Can I Use](https://caniuse.com/?search=MediaDevices%20API) 给出的支持情况：

![MediaDevices API 支持情况](https://upload-images.jianshu.io/upload_images/18281896-e84336d614ba3622.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 访问摄像头示例

简单的 HTML 结构如下：

```html
<button id="start-webcam-btn">开启摄像头</button>
<button id="stop-webcam-btn">停止摄像头</button>

<div id="container">
  <video autoplay="true" id="videoElement"></video>
</div>
```

CSS 如下：

```css
.videoElement {
  width: 500px;
  height: 500px;
  border: 10px solid plum;
}
```

### 开启摄像头

使用 `navigator.mediaDevices.getUserMedia(constraints)` 访问摄像头。将返回的 `stream` 分配给了一个 `<video>` 元素。

```js
var video = document.querySelector('#videoElement')

const start = () => {
  if (navigator.mediaDevices.getUserMedia) {
    navigator.mediaDevices
      .getUserMedia({ video: true })
      .then((stream) => {
        video.srcObject = stream
      })
      .catch((err) => {
        console.log('ERROR: ', err)
      })
  }
}
```

你可以在 👉[查看效果](https://codepen.io/lio-zero/pen/GRWLppK)。

如果你点击了按钮，但没有打开摄像头的话，可能是你禁止了浏览器访问。如果你使用谷歌浏览器，可以在浏览器地址栏最右边的看到如下：

![禁止访问摄像头](https://upload-images.jianshu.io/upload_images/18281896-00f91364d42259ea.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

你可以允许或者禁止浏览器使用您的摄像头。

![允许或者禁止浏览器使用您的摄像头](https://upload-images.jianshu.io/upload_images/18281896-7c70a24933da494e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 停止网络摄像头

```js
const stop = (e) => {
  const stream = video.srcObject
  const tracks = stream.getTracks()

  for (i = 0; i < tracks.length; i++) {
    var track = tracks[i]
    track.stop()
  }

  video.srcObject = null
}
```

## constraints 参数

> 在介绍使用 `mediaDevices.getUserMedia(constraints)` 时，需要了解的一些关于 `constraints` 参数的配置选择。

`constraints` —— 指定了请求的媒体类型和相对应的参数。

以下同时请求不带任何参数的音频和视频：

```js
const constraints = {
  audio: true,
  video: true
}
```

如果为某种媒体类型设置了 `true` ，得到的结果的流中就需要有此种类型的轨道。

如果其中一个由于某种原因无法获得，`getUserMedia()` 将会产生一个错误。

当由于隐私保护的原因，无法访问用户的摄像头和麦克风信息时，应用可以使用额外的 `constraints` 参数请求它所需要或者想要的摄像头和麦克风能力。下面演示了应用想要使用 `1280 x 720` 的摄像头分辨率：

```js
const constraints = {
  audio: true,
  video: { width: 1280, height: 720 }
}
```

浏览器会试着满足这个请求参数，但是如果无法准确满足此请求中参数要求或者用户选择覆盖了请求中的参数时，有可能返回其它的分辨率。

强制要求获取特定的尺寸时，可以使用关键字 `min`，`max` 或者 `exact`（就是 `min == max`）。以下参数表示要求获取最低为 `1280 x 720` 的分辨率。

```js
const constraints = {
  audio: true,
  video: {
    width: { min: 1280 },
    height: { min: 720 }
  }
}
```

如果摄像头不支持请求的或者更高的分辨率，返回的 `Promise` 会处于 `rejected` 状态，而且用户将不会得到要求授权的提示。

造成不同表现的原因是，相对于简单的请求值和 `ideal` 关键字而言，关键字 `min`，`max` 和 `exact` 有着内在关联的强制性，请看一个更详细的例子：

```js
const constraints = {
  audio: true,
  video: {
    width: { min: 1024, ideal: 1280, max: 1920 },
    height: { min: 776, ideal: 720, max: 1080 }
  }
}
```

当请求包含一个 `ideal`（应用最理想的）值时，这个值有着更高的权重，意味着浏览器会先尝试找到最接近指定的理想值的设定或者摄像头（如果设备拥有不止一个摄像头）。

简单的请求值也可以理解为是应用理想的值，因此我们的第一个指定分辨率的请求也可以写成如下：

```js
const constraints = {
  audio: true,
  video: {
    width: { ideal: 1280 },
    height: { ideal: 720 }
  }
}
```

并不是所有的 `constraints` 都是数字。例如，在移动设备上面，如下的例子表示优先使用前置摄像头（如果有的话）：

```js
const constraints = {
  audio: true,
  video: {
    facingMode: 'user'
  }
}
```

强制使用后置摄像头，可以使用：

```js
const constraints = {
  audio: true,
  video: {
    facingMode: {
      exact: 'environment'
    }
  }
}
```

在某些情况下，比如 WebRTC 上使用受限带宽传输时，低帧率可能更适宜。

```js
const constraints = {
  video: {
    frameRate: {
      ideal: 10,
      max: 15
    }
  }
}
```

## 更多资料

[Sketchy Webcam Filter Effects](https://frontend.horse/articles/sketchy-webcam-filter-effects/)
