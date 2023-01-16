# 强制浏览器下载新的 favicon

为了减少加载时间，浏览器通常会缓存静态资源，例如图像、JavaScript、CSS 文件等。favicon 就是其中之一。

一些浏览器，例如 Chrome 浏览器，维护一个单独的存储来缓存网站 favicon，因为它们经常在不同的地方使用，例如书签、历史记录。

当我们用新的 favicon 替换旧的 favicon 时，我们的访问者可能仍然会看到旧的 favicon，即使他们清除了浏览器缓存。

为了防止这种情况发生，我们可以通过添加查询参数来强制浏览器下载新的 favicon。

例如，如果你的 favicon 的原始 URL 为 `example.com/favicon.ico`，可以将其更改为 `example.com/favicon.ico?v=2` 或 `example.com/favicon.ico?t=20221231`，这样浏览器就会认为这是一个新的 favicon，而不是缓存中的版本。

```html
<link rel="icon" href="/favicon.png?v=2" />
```

当然，你可以选择参数的另一个名称和值（如版本号或时间戳）。

此外，更改 HTML 文件中的 favicon 引用路径也可以强制浏览器重新下载 favicon。
