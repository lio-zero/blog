# WebP 图像格式

WebP 是 Google 开发的一种开源图像格式，它承诺生成比 JPG 和 PNG 格式更小的图像，同时生成更好看的图像。

- WebP 支持透明度，如 PNG 和 GIF 图像
- WebP 支持动画，例如 GIF 图像
- 使用 WebP，您可以设置图像的质量比例，决定是要获得更好的质量还是更小的尺寸（就像 JPG 图像中发生的那样）

所以 WebP 可以做所有 GIF、JPG 和 PNG 图像可以做的一切，以单一格式，并生成更小的图像。

如果您想比较各种格式的图像的外观，可以访问 Google 提供的 [WebP 图像示例](https://developers.google.com/speed/webp/gallery)。

我们要知道，WebP 并不算很新的东西，它已经存在了好几年了。

## 对比其他图像格式，WebP 小了多少？

生成较小图像的前提非常有趣，尤其是当您考虑到大部分网页大小取决于用户应下载的图像资源的数量和大小时。

Google 发布了一项对 100 万张图像的结果的比较[研究](https://developers.google.com/speed/webp/docs/c_study)，结果如下：

WebP 总体上比 JPEG 或 JPEG 2000 实现更高的压缩率。对于网页上最常见的小图片来说，文件大小最小化的收益尤其高。

您应该尝试您打算提供的图像类型，并在此基础上形成您的意见。

在我的测试中，无损压缩生成的 WebP 图像比 PNG 小 50%。PNG 只有在使用有损压缩时才会达到这个文件大小。

## 生成 WebP 图像

所有现代图形和图像编辑工具都允许您导出到 `.webp` 文件。

还存在用于将图像直接转换为 WebP 的命令行工具。Google 为此提供了一套[工具](https://developers.google.com/speed/webp/download)。

`cwebp` 是一个命令行程序，用于将任何图像转换为 `webp`:

```txt
cwebp image.png -o image.webp
```

使用 `cwebp -longhelp` 查看所有选项。

## 浏览器支持

[WebP 支持情况](https://caniuse.com/webp)：

![WebP 支持情况](https://upload-images.jianshu.io/upload_images/18281896-651bae1111a408b1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

可以看到，除了 IE，所以主流浏览器都支持 WebP。

## 如何使用 WebP？

有几种不同的方法可以做到这一点。

当 `HTTP_ACCEPT` 请求头包含 `image/webp` 时，您可以使用服务器级的机制来提供 WebP 图像，而不是 JPG 和 PNG。

第一个是最方便的，因为对您和您的网页完全透明。

另一个选择是使用 [Modernizr](https://modernizr.com/) 并检查 `Modernizr.webp` 设置。

如果您不需要支持 IE，一个非常方便的方法是使用 `<picture>` 标签，现在除了 IE（所有版本）之外的所有主流浏览器都支持这个标签。

`<picture>` 标签通常用于响应式图像，但我们也可以将它用于 WebP。

您可以指定图片列表，它们将按顺序使用。以下示例中，支持 WebP 的浏览器将使用第一张图片，如果不支持则回退到 JPG：

```html
<picture>
  <source type="image/webp" srcset="image.webp" />
  <img src="image.jpg" alt="一张图片" />
</picture>
```
