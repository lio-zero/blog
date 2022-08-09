# 使用 srcset 的响应式图像

`img` 标签的 [`srcset` 属性](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLImageElement/srcset)允许您根据你的喜好设置浏览器可以使用的响应图像，具体取决于像素密度或窗口宽度。通过这种方式，它只能下载渲染页面所需的资源，而不需要下载更大的图片（如果它是在移动设备上）。

下面是一个例子，我们为 4 种不同的屏幕尺寸提供了 4 个额外的图像:

```html
<img
  src="avatar.png"
  alt="一张头像"
  srcset="
    avatar-500.png   500w,
    avatar-800.png   800w,
    avatar-1000.png 1000w,
    avatar-1400.png 1400w
  "
/>
```

在 `srcset` 中，我们使用 `w` 度量值来指示窗口宽度。

我们还需要使用 [`sizes` 属性](https://developer.mozilla.org/en-US/docs/Web/API/HTMLImageElement/sizes)，它允许您为每个媒体条件列表指定图像的布局宽度:

```html
<img
  src="avatar.png"
  alt="一张头像"
  sizes="(max-width: 500px) 100vw, (max-width: 900px) 50vw, 800px"
  srcset="
    avatar-500.png   500w,
    avatar-800.png   800w,
    avatar-1000.png 1000w,
    avatar-1400.png 1400w
  "
/>
```

在本例中，`sizes` 属性中的 `(max-width: 500px) 100vw, (max-width: 900px) 50vw, 800px` 字符串描述了图像相对于视口的大小，多个条件用逗号分隔。

媒体条件 `max-width: 500px` 设置与视口宽度相关的图像大小。简而言之，如果窗口大小为 `< 500px`，它将以 100% 的窗口大小渲染图像。

如果窗口大小较大但 `< 900px`，它将以窗口大小的 50% 渲染图像。

如果再大一些，它会渲染 `800px` 的图像。

`vw` 的测量单位表示，`1vw` 是窗口宽度的 `1%`，所以 `100vw` 是窗口宽度的 `100%`。

## 更多资料

- [CSS 单位及其需要注意的地方](https://github.com/lio-zero/blog/blob/main/CSS/CSS%20%E5%8D%95%E4%BD%8D%E5%8F%8A%E5%85%B6%E9%9C%80%E8%A6%81%E6%B3%A8%E6%84%8F%E7%9A%84%E5%9C%B0%E6%96%B9.md)
- [Responsive Image Breakpoints Generator](https://responsivebreakpoints.com/) 网站可以用来生成 `srcset` 和逐渐变小的图片。
- [响应式图像教程](http://www.ruanyifeng.com/blog/2019/06/responsive-images.html)
- [Responsive Images: If you’re just changing resolutions, use srcset.](https://css-tricks.com/responsive-images-youre-just-changing-resolutions-use-srcset/)
- [Responsive Images Done Right: A Guide To `<picture>` And srcset](https://www.smashingmagazine.com/2014/05/responsive-images-done-right-guide-picture-srcset/)
- [Responsive Image Breakpoints Generator, A New Open Source Tool](https://www.smashingmagazine.com/2016/01/responsive-image-breakpoints-generation/)
- [Leaner Responsive Images With Client Hints](https://www.smashingmagazine.com/2016/01/leaner-responsive-images-client-hints/)
- [How To Solve Adaptive Images In Responsive Web Design](https://www.smashingmagazine.com/2013/06/clown-car-technique-solving-for-adaptive-images-in-responsive-web-design/)
- [Picturefill 2.0: Responsive Images And The Perfect Polyfill](https://www.smashingmagazine.com/2014/05/picturefill-2-0-responsive-images-and-the-perfect-polyfill/)
- [Srcset and sizes](https://ericportis.com/posts/2014/srcset-sizes/)
- [Picturefill](https://scottjehl.github.io/picturefill/)
