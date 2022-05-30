# 正确加载 SVG Favicon

SVG Favicon 几乎是跨浏览器支持的。不幸的是，Safari 还不支持，这就是为什么我们仍然需要定义 `ico` favicon。

![2022/2/8：SVG Favicon 支持情况](https://upload-images.jianshu.io/upload_images/18281896-a7514d4e17a64e0c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

使用 `sizes="any"` 属性避免 Chromium 浏览器同时加载 favicon 文件（`ico` 和 `svg`）。

```html
<!-- 避免 Chromium 同时加载 ico 和 svg -->
<link rel="icon" href="/favicon.ico" sizes="any" />
<link rel="icon" href="/favicon.svg" />
```
