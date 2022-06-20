# 使用 loading 属性延迟加载图片

我们经常需要对网站中的图像进行优化，其中一项技术便是懒加载，通常是延迟加载初始视口外的图像，直到我们滚动页面，达到图像与底部视口的交汇处才开始加载图像。

通常情况下，我们都是使用 JS 编写方法（例如 [Intersection Observer API](https://developer.mozilla.org/en-US/docs/Web/API/Intersection_Observer_API) ）或外部库延迟加载图像。而 HTML 标准中已经存在 `loading` 属性，可以精确的感知这种行为。

以下是 [Can I Use](https://caniuse.com/?search=loading) 给出其的兼容情况：

![loading 支持情况](https://upload-images.jianshu.io/upload_images/18281896-ad5bc70865973a6f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

可以看到，在最新版本的 Chromium powered（包括 Chrome、Edge 和 Opera）和 Firefox 浏览器中，都支持 `loading="lazy"` 属性。

用法很简单，我们只需要在想到延迟加载的图像上添加 `loading="lazy"` 属性即可：

```html
<img src="/img/logo.png" alt="website logo" loading="lazy" />
```

浏览器现在将延迟加载图像，直到它们在视口中可见。

当图像完全加载时，通常会看到布局发生移动。为避免此问题，建议使用内联样式或属性设置图像的大小：

```html
<img
  src="/img/logo.png"
  alt="website logo"
  loading="lazy"
  height="200"
  width="300"
/>
```

> [查看效果](https://codepen.io/lio-zero/pen/oNeQKXq)

> **Tips**：您可以打开控制台的 **Network** 选项卡边滚动边查看效果，**注意浏览器版本**。
