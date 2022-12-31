# URL API

[URL API](https://developer.mozilla.org/en-US/docs/Web/API/URL_API) 是 URL 标准的一个组成部分，它定义了什么构成了有效的统一资源定位器，以及提供了解析、构造、规范化和编码 URL 的简单方法的 API。URL 标准还定义了域、主机和 IP 地址等概念。

URL（Uniform Resource Locator）API 是用来处理和解析 URL 的库。

常用的 URL API 有 URL 和 URLSearchParams。

- URL 类用于表示 URL，并提供了许多方法来操作 URL 的各个部分，如协议、主机名、端口号、路径等。
- URLSearchParams 类用于表示 URL 中的查询字符串（即 `?` 后面的部分），并提供了许多方法来操作查询参数。

> 在[使用 URLSearchParams 在 JavaScript 中获取查询字符串值](https://github.com/lio-zero/blog/blob/main/JavaScript/%E4%BD%BF%E7%94%A8%20URLSearchParams%20%E5%9C%A8%20JavaScript%20%E4%B8%AD%E8%8E%B7%E5%8F%96%E6%9F%A5%E8%AF%A2%E5%AD%97%E7%AC%A6%E4%B8%B2%E5%80%BC.md)这篇文章中我们已经有所介绍，下面就不在进行过多讲解。

URL 类属性有：

- `hash` 属性包含 URL 标识中的 `#` 和 fragment 标识符（fragment 即我们通常所说的 URL hash）
- `protocol` 属性可以获取 URL 的协议
- `host` 属性可以获取 URL 的主机名
- `port` 属性可以获取 URL 的端口号
- `pathname` 属性可以获取 URL 的路径
- `searchParams` 属性可以直接访问 URL 的查询字符串（使用 URLSearchParams 处理）
- `href` 属性可以获取或设置整个 URL
- `origin` 属性可以获取 URL 的协议、主机名和端口号
- `searchParams` 属性包含当前 URL 中解码后的 `GET` 查询参数。
- `username`、`password`、`hostname` 属性可以获取或设置 URL 的用户名、密码和主机名（不包括端口号）

但请注意，URL API 仅在浏览器中可用，如果要在 Node.js 中使用，则需要使用 `url` 模块。

以下是使用 URL 类的方法：

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

## 浏览器支持情况

除了 IE 和 Opera Mini，现代浏览器都支持 URL API：

![URL API 的支持情况](https://upload-images.jianshu.io/upload_images/18281896-c7ca63a2133169e4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
