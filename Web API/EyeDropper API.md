# EyeDropper API

[EyeDropper API](https://developer.mozilla.org/en-US/docs/Web/API/EyeDropper_API) 是一种用于在网页上提取颜色的技术。它可以让用户选择屏幕（包括浏览器窗口外）的任何一点，并返回该点的颜色值。这对于设计师或开发人员来说非常有用，因为它可以让他们更轻松地获取网页上的颜色。

在没有实现 EyeDropper API 之前，我们可以使用 Canvas API 来实现获取图像颜色。例如，可以在 `canvas` 上绘制图像，然后使用 `getImageData()` 方法获取图像数据，并使用该数据来提取颜色。

现在，我们可以方便的获取网页上任何位置的颜色，甚至获取浏览器窗口外的颜色。

以下是 EyeDropper API 的简单使用示例：

```js
const eyeDropper = new EyeDropper()

document.getElementById('eyeDropper-btn').addEventListener('click', (e) => {
  eyeDropper
    .open()
    .then((colorSelectionResult) => {
      // 返回所选像素的十六进制颜色值（#RRGGBB）
      console.log(colorSelectionResult.sRGBHex)
    })
    .catch((e) => {
      // 处理用户选择退出 eyedropper 模式而不进行选择
      console.log(e)
    })
})
```

## 浏览器支持情况

EyeDropper API 还处于试验阶段，目前仅在最新的 Chrome、Edge 以及 Opera 浏览器版本中得到支持。

![EyeDropper API 支持情况](https://upload-images.jianshu.io/upload_images/18281896-c460dc823c1a659f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

浏览器兼容性表：[Browser compatibility](https://developer.mozilla.org/en-US/docs/Web/API/EyeDropper_API#browser_compatibility) 或 [Can I use](https://caniuse.com/?search=EyeDropper%20API)。
