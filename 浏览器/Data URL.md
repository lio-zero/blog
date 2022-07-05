# Data URL

[Data URL](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Basics_of_HTTP/Data_URIs) 是一种 URI 方案，它提供了一种在 HTML 文档中内联数据的方法。

假设你想嵌入一个小图像。您可以按照通常的方式，将其上传到文件夹，并使用 `img` 标签使浏览器从网络中引用它：

```html
<img src="image.png" />
```

或者，您可以将其编码为一种特殊的格式，称为 Data URL，这样就可以将图像直接嵌入 HTML 文档中，因此浏览器不必单独请求即可获取图像。

Data URL 可能会为小文件节省一些时间，但对于较大的文件，HTML 文件大小的增加会带来不利影响，如果图像重新加载到所有页面上，这尤其是一个问题，因为您无法利用浏览器的缓存功能。

此外，编码为 Data URL 的图像通常比二进制格式的相同图像大一点。

如果您的图像经常被编辑，它们就不太实用，因为每次更改图像时，都必须再次对其进行编码。

## Data URL 看起来如何？

Data URL 是一个以 `data:MIME` 类型格式开头的字符串。例如，PNG 图像具有 [MIME](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Basics_of_HTTP/MIME_Types) 类型 `image/png`。

> [常见的 MIME 类型列表](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Basics_of_HTTP/MIME_types/Common_types)

后面是逗号，然后是实际数据。

文本通常以纯文本传输，而二进制数据通常采用 base64 编码。

以下是此类 Data URL 的示例：

```html
<img src="data:image/png,%89PNG%0D%0A..." />
```

下面是 base64 编码的 Data URL 的样子。请注意，它以 `data:image/png;base64` 开始：

```html
<img src="data:image/png;base64,iVBORw0KGgoAA..." />
```

一个不错的 [Data URL 生成器](https://dopiaza.org/tools/datauri/index.php)，您可以使用它将任何图像转换为 Data URL。

Data URL 可以在任何可以使用 URL 的地方使用，如您所见，您可以将其用于链接，但在 CSS 中使用它们也很常见：

```css
.main {
  background-image url('data:image/png;base64,iVBORw0KGgoAA...');
}
```

## 浏览器支持

所有现代浏览器都支持它们。

![Data URL 支持情况](https://upload-images.jianshu.io/upload_images/18281896-c12164199f5682ec.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 安全

Data URL 可以对任何类型的信息进行编码，而不仅仅是图像，因此它们具有一组安全隐患。

来自维基百科：

> Data URI 可用于构建攻击页面，试图从毫无戒心的 Web 用户那里获取用户名和密码。它还可以用于绕过[跨站点脚本（XSS）](https://github.com/lio-zero/blog/blob/main/WTF/%E4%BB%80%E4%B9%88%E6%98%AF%20XSS%20%E6%94%BB%E5%87%BB%EF%BC%9F.md)限制，将攻击负载完全嵌入地址栏中，并通过 URL 缩短服务托管，而不需要由第三方控制的完整网站。

查看 [Blocking Top-Level Navigations to data URLs for Firefox 59](https://blog.mozilla.org/security/2017/11/27/blocking-top-level-navigations-data-urls-firefox-59/) 以了解如何降低被攻击风险。

## 总结

Data URL 的语法：

```txt
data:[<media type>][;charset=<character set>][;base64],<data>
```

Data URL 由四部分组成：

- 前缀 `data:`
- 指示数据类型的 MIME 类型。例如 `image/jpeg` 表示 JPEG 图像文件；如果此部分被省略，则默认值为 `text/plain;charset=US-SACII`
- 如果为非文本数据，则可选 base64 做标记
- 数据

优缺点：

- **优点** — 我们可以将图片转化为 base64 直接使用，而无需额外的发送请求图片。
- **缺点**：
  - base64 编码后的图片会比原来的体积大三分之一左右。
  - Data URL 形式的图片不会缓存下来，每次访问页面都要被下载一次。但我们可以将 Data URL 写入到 CSS 文件中随着 CSS 被缓存下来。

Data URL 在 [RFC 2397](https://datatracker.ietf.org/doc/html/rfc2397) 中定义。
