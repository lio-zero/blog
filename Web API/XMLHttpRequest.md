# XMLHttpRequest

XMLHttpRequest（XHR）是微软在九十年代发明的，在 2002-2006 年间，随着所有浏览器的实现，XHR 成为了标准。2006 年，W3C 标准化了 XMLHttpRequest。

由于这种情况有时会发生在 Web 平台上，最初存在一些不一致，使得跨浏览器使用 XHR 变得截然不同。

像 jQuery 这样的库通过为开发人员提供易于使用的抽象而得到了普及，这反过来又帮助推广了这种技术的使用。

## XHR 请求示例

以下代码创建一个 XMLHttpRequest (XHR) 请求对象，并附加一个响应 `onreadystatechange` 事件的回调函数。

设置 xhr 连接是为了执行对 `https://yoursite.com` 的 GET 请求，并使用 `send()` 方法发送：

```js
const xhr = new XMLHttpRequest()
xhr.onreadystatechange = () => {
  if (xhr.readyState === 4) {
    xhr.status === 200 ? console.log(xhr.responseText) : console.error('error')
  }
}
xhr.open('GET', 'https://yoursite.com')
xhr.send()
```

## 其他 `open()` 参数

在上面的示例中，我们只是将方法和 URL 传递给请求。

我们还可以指定其他 HTTP 方法（`get`、`post`、`head`、`put`、`delete` 和 `options`）。

其他参数允许您指定一个标志，以便在设置为 `false` 时使请求同步，以及一组用于 HTTP 身份验证的凭据：

```js
open(method, url, asynchronous, username, password)
```

### `onreadystatechange`

在 XHR 请求过程中多次调用 `onreadystatechange`。我们显式忽略除 `readyState === 4` 之外的所有状态，这意味着请求已完成。

状态是：

- 1（OPEN）：请求开始
- 2 (HEADERS_RECEIVED)：HTTP header 已被接收
- 3（LOADING）：响应开始下载
- 4 (DONE)：响应已下载

## 中止 XHR 请求

可以通过调用 XHR 对象上的 `abort()` 方法中止 XHR 请求。

## 与 jQuery 的比较

使用 jQuery，这些行可以转换为：

```js
$.get('https://yoursite.com', (data) => {
  console.log(data)
}).fail((err) => {
  console.error(err)
})
```

## 与 Fetch 的比较

使用 Fetch API，这是等效的代码：

```js
fetch('https://yoursite.com')
  .then((data) => {
    console.log(data)
  })
  .catch((err) => {
    console.error(err)
  })
```

## 跨域请求

请注意，XMLHttpRequest 连接受到出于安全原因而强制执行的特定限制的约束。

其中最明显的一项是[同源策略](https://github.com/lio-zero/blog/blob/main/%E6%B5%8F%E8%A7%88%E5%99%A8/%E6%B5%8F%E8%A7%88%E5%99%A8%E5%90%8C%E6%BA%90%E7%AD%96%E7%95%A5.md)的执行。

您无法访问另一台服务器上的资源，除非该服务器使用 CORS（跨源资源共享）明确支持这一点。

## 更多资料

- [XMLHttpRequest](https://zh.javascript.info/xmlhttprequest)
- [XMLHttpRequest Level 2 使用指南](https://www.ruanyifeng.com/blog/2012/09/xmlhttprequest_level_2.html)
- [实现一个 AJAX HTTP 请求库](https://github.com/lio-zero/blog/blob/main/%E6%89%8B%E5%86%99%E7%B3%BB%E5%88%97/%E5%AE%9E%E7%8E%B0%E4%B8%80%E4%B8%AA%20Ajax%20HTTP%20%E8%AF%B7%E6%B1%82%E5%BA%93.md)
