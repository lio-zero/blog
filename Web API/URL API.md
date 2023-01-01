# URL API

[URL API](https://developer.mozilla.org/en-US/docs/Web/API/URL_API) 是 URL 标准的一个组成部分，它定义了什么构成了有效的统一资源定位器，以及提供了解析、构造、规范化和编码 URL 的简单方法的 API。URL 标准还定义了域、主机和 IP 地址等概念。

URL（Uniform Resource Locator）API 是用来处理和解析 URL 的库。

常用的 URL API 有 URL 和 URLSearchParams。

- URL 类用于表示 URL，并提供了许多方法来操作 URL 的各个部分，如协议、域名、端口号、路径等。
- URLSearchParams 类用于表示 URL 中的查询字符串（即 `?` 后面的部分），并提供了许多方法来操作查询参数。

> 在[使用 URLSearchParams 在 JavaScript 中获取查询字符串值](https://github.com/lio-zero/blog/blob/main/JavaScript/%E4%BD%BF%E7%94%A8%20URLSearchParams%20%E5%9C%A8%20JavaScript%20%E4%B8%AD%E8%8E%B7%E5%8F%96%E6%9F%A5%E8%AF%A2%E5%AD%97%E7%AC%A6%E4%B8%B2%E5%80%BC.md)这篇文章中我们已经有所介绍，下面就不在进行过多讲解。

现在 `currentUrl` 有一组可用于检查 URL 的属性：

URL 类提供了一组可用于检查 URL 的属性：

- `hash` 属性包含 URL 标识中的 `#` 和 fragment 标识符（fragment 即我们通常所说的 URL hash）
- `protocol` 属性可以获取 URL 的协议
- `host` 属性可以获取 URL 的域名（包括端口）
- `port` 属性可以获取 URL 的端口号
- `pathname` 属性可以获取 URL 的路径
- `searchParams` 属性可以直接访问 URL 的查询字符串（使用 URLSearchParams 处理）
- `href` 属性可以获取或设置整个 URL
- `origin` 属性可以获取 URL 的协议、域名和端口号
- `username`、`password`、`hostname` 属性可以获取或设置 URL 的用户名、密码和域名（不包括端口号）

除了 `origin` 和 `searchParams` 是只读属性外，你可以更改其中的任何一个，并通过调用 `toString()` 方法或通过引用 `href` 属性生成新的 URL 字符串。

但请注意，URL API 仅在浏览器中可用，如果要在 Node.js 中使用，则需要使用 `url` 模块。

以上 URL 属性完整介绍可以在左侧点击查看 👉 [完整列表](https://developer.mozilla.org/zh-CN/docs/Web/API/URL#%E5%B1%9E%E6%80%A7)。

以下是使用 URL 类属性的简单示例：

```js
const url = new URL('https://www.example.com/path?key=value')

url.protocol // 'https:'
url.host // 'www.example.com'
url.port // '' => 如果未指定端口号，则为空字符串
url.pathname // '/path'
// ...

// 获取 URL 中的查询字符串（使用 URLSearchParams 处理）
const searchParams = new URLSearchParams(url.search)

// 获取查询参数
searchParams.get('key') // 'value'

// 设置查询参数
searchParams.set('key', 'newValue')

// 重新设置 URL 的查询字符串
url.search = searchParams.toString()
```

## 静态方法

[URL](https://developer.mozilla.org/zh-CN/docs/Web/API/URL) 对象提供了 2 个静态方法，使用 Blob 操作 URL：

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

关于以上 URL 对象提供的两个静态方法，有几个不错的使用案例：

- [从剪贴板粘贴图像](https://github.com/lio-zero/blog/blob/main/JavaScript/%E4%BB%8E%E5%89%AA%E8%B4%B4%E6%9D%BF%E7%B2%98%E8%B4%B4%E5%9B%BE%E5%83%8F.md)
- [调整图像大小](https://github.com/lio-zero/blog/blob/main/JavaScript/%E8%B0%83%E6%95%B4%E5%9B%BE%E5%83%8F%E5%A4%A7%E5%B0%8F.md)
- [将 JSON 数据下载为文件](https://github.com/lio-zero/blog/blob/main/JavaScript/%E5%B0%86%20JSON%20%E6%95%B0%E6%8D%AE%E4%B8%8B%E8%BD%BD%E4%B8%BA%E6%96%87%E4%BB%B6.md)
- [前端文件上传-用对象处理文件](https://github.com/lio-zero/blog/blob/main/JavaScript/%E5%89%8D%E7%AB%AF%E6%96%87%E4%BB%B6%E4%B8%8A%E4%BC%A0.md#%E7%94%A8%E5%AF%B9%E8%B1%A1%E5%A4%84%E7%90%86%E6%96%87%E4%BB%B6)
- [下载文件](https://github.com/lio-zero/blog/blob/main/JavaScript/%E4%B8%8B%E8%BD%BD%E6%96%87%E4%BB%B6.md)
- [MDN：在 web 应用程序中使用文件](https://developer.mozilla.org/zh-CN/docs/Web/API/File_API/Using_files_from_web_applications)

## 浏览器支持情况

除了 IE 和 Opera Mini，现代浏览器都支持 URL API：

![URL API 的支持情况](https://upload-images.jianshu.io/upload_images/18281896-c7ca63a2133169e4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
