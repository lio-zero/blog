# URL 对象

[URL](https://developer.mozilla.org/zh-CN/docs/Web/API/URL) 是一个命名空间，用于托管 2 个静态方法，使用 Blob 操作 URL：

- `URL.createObjectURL()`
- `URL.revokeObjectURL()`

给定一个 blob，您可以使用 `URL.createObjectURL()` 函数生成一个 URL：

```js
const myURL = URL.createObjectURL(aBlob)
```

一旦您拥有 blob URL，您可以使用以下方法从内存中销毁它：

```js
URL.revokeObjectURL(myURL)
```

除此之外，URL 通过其构造函数提供了非常不同的功能，可以用来创建 URL。你可以这样调用它：

```js
const currentUrl = new URL(window.location.href)
```

现在 `currentUrl` 有一组可用于检查 URL 的属性：

- `hash` 哈希片段
- `host` 域名 + 端口
- `hostname` 域名
- `href` 包含整个 URL
- `origin` 协议、域名和端口
- etc

这是 URL 的常用部分。点击这里查看[完整列表](https://developer.mozilla.org/zh-CN/docs/Web/API/URL#%E5%B1%9E%E6%80%A7)。

除了 `origin` 和 `searchParams` 是只读属性外，您可以更改其中的任何一个，并通过调用 `toString()` 方法或通过引用 `href` 属性生成新的 URL 字符串。

关于 URL 对象，有几个不错的示例可供参考：

- [从剪贴板粘贴图像](https://github.com/lio-zero/blog/blob/main/JavaScript/%E4%BB%8E%E5%89%AA%E8%B4%B4%E6%9D%BF%E7%B2%98%E8%B4%B4%E5%9B%BE%E5%83%8F.md)
- [调整图像大小](https://github.com/lio-zero/blog/blob/main/JavaScript/%E8%B0%83%E6%95%B4%E5%9B%BE%E5%83%8F%E5%A4%A7%E5%B0%8F.md)
- [将 JSON 数据下载为文件](https://github.com/lio-zero/blog/blob/main/JavaScript/%E5%B0%86%20JSON%20%E6%95%B0%E6%8D%AE%E4%B8%8B%E8%BD%BD%E4%B8%BA%E6%96%87%E4%BB%B6.md)
- [前端文件上传-用对象处理文件](https://github.com/lio-zero/blog/blob/main/JavaScript/%E5%89%8D%E7%AB%AF%E6%96%87%E4%BB%B6%E4%B8%8A%E4%BC%A0.md#%E7%94%A8%E5%AF%B9%E8%B1%A1%E5%A4%84%E7%90%86%E6%96%87%E4%BB%B6)
- [下载文件](https://github.com/lio-zero/blog/blob/main/JavaScript/%E4%B8%8B%E8%BD%BD%E6%96%87%E4%BB%B6.md)
- [MDN：在 web 应用程序中使用文件](https://developer.mozilla.org/zh-CN/docs/Web/API/File_API/Using_files_from_web_applications)
