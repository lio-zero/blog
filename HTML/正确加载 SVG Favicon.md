# 正确加载 SVG Favicon

SVG Favicon 几乎是跨浏览器支持的。不幸的是，Safari 还不支持，这就是为什么我们仍然需要定义 `ico` favicon。

![2022/2/8：SVG Favicon 支持情况](https://upload-images.jianshu.io/upload_images/18281896-a7514d4e17a64e0c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

使用 `sizes="any"` 属性避免 Chromium 浏览器同时加载 favicon 文件（`ico` 和 `svg`）。

```html
<!-- 避免 Chromium 同时加载 ico 和 svg -->
<link rel="icon" href="/favicon.ico" sizes="any" />
<link rel="icon" href="/favicon.svg" />
```

## 更多资料

- [强制浏览器下载新的 favicon](https://github.com/lio-zero/blog/blob/main/HTML/%E5%BC%BA%E5%88%B6%E6%B5%8F%E8%A7%88%E5%99%A8%E4%B8%8B%E8%BD%BD%E6%96%B0%E7%9A%84%20favicon.md)
- [We Analyzed 425,909 Favicons](https://iconmap.io/blog#before-we-begin---favicon-best-practices)
- [Definitive edition of "How to Favicon in 2021"](https://dev.to/masakudamatsu/favicon-nightmare-how-to-maintain-sanity-3al7)
- [创建自动更改的 Favicon](https://css-tricks.com/how-to-create-a-favicon-that-changes-automatically/)
