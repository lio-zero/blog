# ImageCapture API

[ImageCapture API](https://developer.mozilla.org/en-US/docs/Web/API/ImageCapture) 接口提供了从相机或其他摄影设备捕获图像或照片的方法。

ImageCapture API 允许网页应用程序访问和控制用户的计算机上的摄像头。这使得开发人员创建的网页应用程序，可以让用户拍摄照片或视频，并将其上传到网页或将其发送到后端服务器。

## 获取权限

要使用 ImageCapture API，首先需要让用户授权访问其摄像头。这可以通过使用 `navigator.permissions.query()` 方法来完成。例如：

```js
navigator.permissions.query({ name: 'camera' }).then(function (result) {
  if (result.state == 'granted') {
    // 用户已授权访问摄像头
  } else if (result.state == 'prompt') {
    // 用户尚未授权访问摄像头，需要请求授权
  } else if (result.state == 'denied') {
    // 用户拒绝了访问摄像头的请求
  }
})
```

## 访问摄像头

如果用户已授权访问摄像头，则可以使用 `navigator.mediaDevices.getUserMedia()` 方法获取摄像头的流。例如：

```js
navigator.mediaDevices.getUserMedia({ video: true }).then((stream) => {
  // 将流播放到 video 元素上
  const videoElement = document.getElementById('video')
  videoElement.srcObject = stream
})
```

然后，可以使用 ImageCapture 构造函数来创建一个新的 ImageCapture 对象，该对象可以用于访问和控制摄像头。例如：

```js
const imageCapture = new ImageCapture(stream.getVideoTracks()[0])
```

ImageCapture 对象提供了许多方法，可用于拍摄照片、获取摄像头的信息和设置摄像头的参数。具体来说，可以使用以下方法：

- `captureFrame` 方法返回一个包含当前摄像头帧的图像数据的 ImageBitmap 对象。
- `grabFrame()` 方法可以获取摄像头的当前帧并返回一个 ImageBitmap 对象。
- `takePhoto()` 方法拍摄一张照片并返回一个包含照片数据的 Blob 对象。
- `getCapabilities()` 方法返回一个对象，其中包含有关摄像头功能的信息，如光圈、快门速度和 ISO 的范围。
- `getPhotoCapabilities()` 方法返回一个对象，其中包含有关摄像头拍摄照片功能的信息，如分辨率、图像质量和对焦模式的范围。
- `setOptions()` 方法设置摄像头的参数，如光圈、快门速度和 ISO。

> ImageBitmap 对象是一种图像数据类型，它可以高效地处理和显示图像。它可以用于在网页中显示图像，或将图像数据发送到后端服务器进行处理。

下面是一个示例，展示了如何使用这些方法：

```js
// 拍摄照片
imageCapture.takePhoto().then((blob) => {
  // 将照片显示在 img 元素上
  const imgElement = document.getElementById('img')
  imgElement.src = URL.createObjectURL(blob)
})

// 获取摄像头功能信息
imageCapture.getCapabilities().then((capabilities) => {
  console.log('摄像头功能：', capabilities)
})

// 设置摄像头参数
imageCapture.setOptions({
  imageHeight: 480,
  imageWidth: 640,
  iso: 100
})
```

如果需要获取摄像头的当前帧，还可以：

```js
// 获取摄像头的当前帧
imageCapture.grabFrame().then((imageBitmap) => {
  // 将图像数据显示在 canvas 元素上
  const canvas = document.getElementById('canvas')
  const context = canvas.getContext('2d')
  context.drawImage(imageBitmap, 0, 0)
})
```

需要注意，使用 `grabFrame` 方法时，需要保证摄像头流正在播放。如果摄像头流已停止，则会返回一个 rejected Promise。

此外，`grabFrame` 方法还可以接受一个可选的参数，用于设置图像的分辨率。这里不在赘述。

[一个 ImageCapture API 的演示地址](https://codepen.io/lio-zero/pen/OJvyqdK)

## 浏览器的支持情况

[ImageCapture API 支持情况](https://caniuse.com/mdn-api_imagecapture)：

![ImageCapture API 支持情况](https://upload-images.jianshu.io/upload_images/18281896-9654b88c3f3e8f52.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

还需要注意，ImageCapture API 仅在支持 [`getUserMedia()` API](<https://github.com/lio-zero/blog/blob/main/Web%20API/getUserMedia()%20%E6%96%B9%E6%B3%95.md>) 的浏览器中可用，这意味着仅在现代浏览器中才能使用它。此外，还需要注意，ImageCapture API 仅能访问和控制摄像头，而不能访问摄像头的音频流。如果需要访问摄像头的音频流，可以使用 [MediaStream API](https://developer.mozilla.org/en-US/docs/Web/API/MediaStream)。

## 最后

ImageCapture API 可以让你在网页中访问和控制摄像头，从而为用户提供更丰富的体验。
