# HTML iframe 标签

[`iframe` 标签](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/iframe)允许我们将来自其他来源（其他网站）的内容嵌入到我们的网页中。

从技术上讲，`iframe` 创建了一个新的嵌套浏览上下文。这意味着 `iframe` 中的任何内容都不会干扰父页面，反之亦然。JavaScript 和 CSS 不会泄漏到 `iframe` 或从 `iframe` 中泄漏。

许多网站使用 `iframe` 执行各种操作。您可能熟悉 [CodePen](https://codepen.io/)、[Glitch](https://glitch.com/)、[CodeSandbox](https://codesandbox.io/) 或其他允许您在页面的某个部分编码的网站，并在一个框中看到结果。那个框便是 `iframe`。

您可以这样创建：

```html
<iframe src="page.html" />
```

您也可以加载绝对 URL：

```html
<iframe src="https://example.com/page.html" />
```

您可以设置一组 `width` 和 `height` 参数（或使用 CSS 设置），否则 `iframe` 将使用默认值，即 `300 x 150` 像素的框：

```html
<iframe src="page.html" width="800" height="400" />
```

## Srcdoc

`srcdoc` 属性允许您指定一些要显示的内联 HTML。它是 `src` 的替代品，[但完全不支持 IE 浏览器](https://caniuse.com/?search=Srcdoc)：

```html
<iframe srcdoc="<p>Hello iframe!</p>" />
```

## Sandbox

`sandbox` 属性允许我们限制 `iframe` 中允许的操作。

如果我们省略它，一切都是允许的：

```html
<iframe src="page.html" />
```

如果我们将其设置为 `''`（空），则不允许：

```html
<iframe src="page.html" sandbox="" />
```

我们可以通过在 `sandbox` 属性中添加选项来选择允许的内容。您可以通过在中间添加空格来允许多个。以下是您可以使用的选项的不完整列表：

- `allow-forms` 允许提交表单
- `allow-modals` 允许打开模态窗口，包括在 JavaScript 调用 `alert()`
- `allow-orientation-lock` 允许锁定屏幕方向
- `allow-popups` 允许弹出窗口、使用 `window.open()` 和 `target="_blank"` 链接
- `allow-same-origin` 将正在加载的资源视为同一来源
- `allow-scripts` 让加载的 `iframe` 运行脚本（但不创建弹出窗口）。
- `allow-top-navigation` 允许访问 `iframe` 以访问顶级浏览上下文

## Allow

目前只是实验性的，并且只支持基于 Chromium 的浏览器，这是父窗口和 `iframe` 之间资源共享的未来。

它类似于 `sandbox` 属性，但允许我们使用特定的功能，包括：

- `accelerometer` 提供对 [Sensors API Accelerometer](https://developer.mozilla.org/en-US/docs/Web/API/Accelerometer) 接口的访问权限
- `ambient-light-sensor` 提供对 [Sensors API AmbientLightSensor](https://developer.mozilla.org/en-US/docs/Web/API/AmbientLightSensor) 接口的访问权限
- `autoplay` 允许自动播放视频和音频文件
- `camera` 允许从 [getUserMedia API](<https://github.com/lio-zero/blog/blob/main/Web%20API/getUserMedia()%20%E6%96%B9%E6%B3%95.md>) 访问相机
- `display-capture` 允许使用 [getDisplayMedia API](https://developer.mozilla.org/en-US/docs/Web/API/MediaDevices/getDisplayMedia) 访问屏幕内容
- `fullscreen` 允许访问全屏模式
- `geolocation` 允许访问 [Geolocation API](https://github.com/lio-zero/blog/blob/main/Web%20API/Web%20Geolocation%20API.md)
- `gyroscope` 提供对 [Sensors API Gyroscope](https://developer.mozilla.org/en-US/docs/Web/API/Gyroscope) 接口的访问权限
- `magnetometer` 提供对 [Sensors API Magnetometer](https://developer.mozilla.org/en-US/docs/Web/API/Magnetometer) 接口的访问权限
- `microphone` 使用 getUserMedia API 访问设备麦克风
- `midi` 允许访问 [Web MIDI API](https://developer.mozilla.org/en-US/docs/Web/API/Web_MIDI_API)
- `payment` 提供对 [Payment Request API](https://developer.mozilla.org/en-US/docs/Web/API/Payment_Request_API) 的访问权限
- `speaker` 允许通过设备扬声器播放音频
- `usb` 提供对 [WebUSB API](https://developer.mozilla.org/en-US/docs/Web/API/WebUSB_API) 的访问权限
- `vibrate` 提供对 [Vibration API](https://github.com/lio-zero/blog/blob/main/Web%20API/Web%20Vibration%20API.md) 的访问权限
- `vr` 提供对 [WebVR API](https://developer.mozilla.org/en-US/docs/Web/API/WebVR_API) 的访问权限

这些都是实验性，完整内容查看 👉 [特征策略](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Feature_Policy)。

## Referrer

加载 `iframe` 时，浏览器会在 `Referer` 标题中向其发送有关谁正在加载它的重要信息（请注意单个 `r`，我们必须忍受的拼写错误）。

> [wiki](https://zh.wikipedia.org/wiki/HTTP%E5%8F%83%E7%85%A7%E4%BD%8D%E5%9D%80#.E6.8B.BC.E5.86.99.E9.97.AE.E9.A2.98)：Referer 的正确英语拼法是 referrer。这是早期 HTTP 规范当中存在的拼写错误，后来为保持向下兼容将错就错。

[`referrerpolicy` 属性](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/iframe#attr-referrerpolicy)允许我们设置在加载时发送到 `iframe` 的 referrer。referrer 是一个 HTTP 头，让页面知道谁在加载它。以下是允许的值：

- `no-referrer` 不发送 referrer 标头
- `no-referrer-when-downgrade` 是默认值，即当前页面通过 HTTPS 加载，并且 `iframe` 在 HTTP 协议上加载时发送 referrer 来源网址
- `origin` 发送 referrer 来源网址，仅包含来源（端口、协议和域），而不包含默认来**源和路径**
- `origin-when-cross-origin` 当从 `iframe` 中的同源（端口、协议、域）加载时，referrer 以其完整形式（来源 + 路径）发送。否则只发送原点
- `same-origin` 仅当从 `iframe` 中的同源（端口、协议、域）加载时，才会发送 referrer 来源网址
- `strict-origin` 如果当前页面通过 HTTPS 加载，并且 `iframe` 也通过 HTTPS 协议加载，那么发送源作为 referrer。如果 `iframe` 通过 HTTP 加载，则不发送任何内容
- `strict-origin-when-cross-origin` 在同源上工作时，将**来源和路径**作为 referrer 发送。如果当前页面通过 HTTPS 加载，并且 `iframe` 也通过 HTTPS 协议加载，那么发送源作为 referrer。如果 `iframe` 通过 HTTP 加载，则不发送任何内容
- `unsafe-url` 即使从 HTTP 加载资源，并且当前页面是通过 HTTPS 加载的，也会将**源和路径**作为 referrer 发送

## 更多资料

- [iframe 是什么？有什么优缺点？](https://github.com/lio-zero/blog/blob/main/HTML/HTML%20HTML5%20%E5%9F%BA%E7%A1%80%E7%9F%A5%E8%AF%86%20%E2%80%94%20%E9%9D%A2%E8%AF%95%E9%A2%98%E4%B8%93%E7%94%A8.md#iframe-%E6%98%AF%E4%BB%80%E4%B9%88%E6%9C%89%E4%BB%80%E4%B9%88%E4%BC%98%E7%BC%BA%E7%82%B9)
- [Making Embedded Content Work In A Responsive iFrame](https://www.smashingmagazine.com/2014/02/making-embedded-content-work-in-responsive-design/)
