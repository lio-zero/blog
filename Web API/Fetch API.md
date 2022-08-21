# Fetch API

自 1998 年发布 IE5 以来，我们可以选择使用 [XMLHttpRequest](https://github.com/lio-zero/blog/blob/main/Web%20API/XMLHttpRequest.md)（XHR）在浏览器中进行异步网络调用。

几年后，GMail 和其他应用大量使用它，并使这种方法如此流行，以至于它不得不有一个名字：AJAX。

直接使用 XMLHttpRequest 一直很痛苦，一些开发者将它抽象成库，尤其是 jQuery 围绕它构建了自己的辅助函数：

- `jQuery.ajax()`
- `jQuery.get()`
- `jQuery.post()`
- etc

它们在使其更易于访问方面产生了巨大的影响，特别是在确保所有功能都能在较旧的浏览器上运行。

[Fetch API](https://developer.mozilla.org/zh-CN/docs/Web/API/Fetch_API) 已被标准化为异步网络请求的现代方法，并使用 Promises 作为构建块。

以下是目前[浏览器对 Fetch API 的支持](https://caniuse.com/?search=Fetch%20API)：

![Fetch API 的支持情况](https://upload-images.jianshu.io/upload_images/18281896-5ddf552173e8d102.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

可以看到，Fetch 对除 IE 以外的主流浏览器都有很好的支持。

GitHub 发布的 [fetch polyfill](https://github.com/github/fetch) 允许我们在任何浏览器上使用 fetch。

## 使用 Fetch

使用 Fetch 对 GET 请求非常简单：

```js
fetch('/file.json')
```

fetch 将发出一个 HTTP 请求来获取 `file.json` 同域上的资源。

如您所见，`fetch` 函数在全局 `window` 范围内可用。

> Tips：您可以使用 [JSONPlaceholder](http://jsonplaceholder.typicode.com/) 提供的 API 进行测试。

现在让我们让它更有用一点，让我们看看文件的内容是什么：

```js
fetch('./file.json')
  .then((response) => response.json())
  .then((data) => console.log(data))
```

调用 `fetch()` 将返回一个 promise。然后，我们可以通过传递带有 promise 的 `then()` 方法的处理程序来等待 promise 解析。

该处理程序接收 fetch promise 的返回值，这是一个 Response 对象。

我们将在下一节中详细了解这个对象。

### 捕捉错误

由于 `fetch()` 返回了一个 promise，我们可以使用 promise 的 `catch` 方法来拦截请求执行期间发生的任何错误，以及在 `then` 回调中完成的处理：

```js
fetch('./file.json')
  .then((response) => {
    // ...
  })
  .catch((err) => console.error(err))
```

捕获错误的另一种方法是在第一个 `then` 中管理错误：

```js
fetch('./file.json')
  .then((response) => {
    if (!response.ok) {
      throw Error(response.statusText)
    }
    return response
  })
  .then((response) => {
    // ...
  })
```

## 响应对象

`fetch()` 调用返回的响应对象包含有关请求和网络请求响应的所有信息。

以下是一些元数据：

### headers

通过访问 `response` 对象上的 `headers` 属性，可以查看请求返回的 HTTP 头：

```js
fetch('./file.json').then((response) => {
  console.log(response.headers.get('Content-Type'))
  console.log(response.headers.get('Date'))
})
```

![获取请求头](https://upload-images.jianshu.io/upload_images/18281896-d177f7513aef13f1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### status

`status` 属性是一个整数，表示 HTTP 响应状态。

例如：

- 200-299 为成功状态
- 301、302、303、307 或 308 是重定向

```js
fetch('./file.json').then((response) => console.log(response.status))
```

> 推荐：[常见的 HTTP 状态码](https://github.com/lio-zero/blog/blob/main/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%BD%91%E7%BB%9C/%E5%B8%B8%E8%A7%81%E7%9A%84%20HTTP%20%E7%8A%B6%E6%80%81%E7%A0%81.md)

### statusText

`statusText` 是表示响应状态消息的属性。如果请求成功，则状态为 OK。

```js
fetch('./file.json').then((response) => console.log(response.statusText))
```

### url

url 表示我们获取的属性的完整 URL。

```js
fetch('./file.json').then((response) => console.log(response.url))
```

### 正文内容

响应有一个正文，可以使用多种方法访问：

- `text()` 以字符串形式返回正文
- `json()` 将正文作为 JSON 解析的对象返回
- `blob()` 将正文作为 Blob 对象返回
- `formData()` 将正文作为 FormData 对象返回
- `arrayBuffer()` 将正文作为 ArrayBuffer 对象返回

所有这些方法都返回一个 Promise。例如：

```js
fetch('./file.json')
  .then((response) => response.text())
  .then((body) => console.log(body))

fetch('./file.json')
  .then((response) => response.json())
  .then((body) => console.log(body))
```

![获取 JSON 数据](https://upload-images.jianshu.io/upload_images/18281896-5ac04e422151a768.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 获取 JSON

也可以使用 ES8 的异步函数编写相同的内容：

```js
;(async () => {
  const response = await fetch('./file.json')
  const data = await response.json()
  console.log(data)
})()
```

## 请求对象

Request 对象表示一个资源请求，通常使用 `new Request()` API 创建。

例如：

```js
const req = new Request('/api/todos')
```

Request 对象提供了几个只读属性来检查资源请求的详细信息，包括：

- `method` — 请求的方法（GET、POST 等）
- `url` — 请求的 URL
- `headers` — 请求的关联 headers 对象
- `referrer` — 请求的引用者
- `cache` — 请求的缓存模式（例如，`default`、`reload` 和 `no-cache`）

并公开了几种方法，包括 `json()`、`text()` 和 `formData()` 来处理请求的正文。

> [完整的 API 列表](https://developer.mozilla.org/docs/Web/API/Request)

## 请求头

能够设置 HTTP 请求头是至关重要的，并且 fetch 使我们能够使用 Headers 对象完成这项工作：

```js
const headers = new Headers()
headers.append('Content-Type', 'application/json')
```

或者：

```js
const headers = new Headers({
  'Content-Type': 'application/json'
})
```

要将 headers 附加到请求，我们使用 Request 对象，并将其传递给 `fetch()`，而不是传递 URL。

```js
fetch('./file.json')
```

替换为：

```js
const request = new Request('./file.json', {
  headers: new Headers({
    'Content-Type': 'application/json'
  })
})

fetch(request)
```

Headers 对象不限于设置值，但我们也可以查询它：

```js
headers.has('Content-Type')
headers.get('Content-Type')
```

我们可以删除之前设置的 header：

```js
headers.delete('X-My-Custom-Header')
```

## POST 请求

Fetch 还允许在请求中使用任何其他 HTTP 方法：POST、PUT、DELETE 或 OPTIONS 等。

在请求的 `method` 属性中指定方法，并在请求头和请求体中传递其他参数：

POST 请求示例：

```js
const options = {
  method: 'post',
  headers: {
    'Content-type': 'application/x-www-form-urlencoded; charset=UTF-8'
  },
  body: 'name=O.O&test=1'
}

fetch(url, options).catch((err) => {
  console.error('Request failed', err)
})
```

## 如何取消获取请求

在引入 fetch 之后的几年中，一旦打开请求，就无法中止。

现在，由于引入了 `AbortController` 和 `AbortSignal`，我们可以使用通用 API 来通知中止事件。

通过将 `signal` 作为 fetch 参数传递来集成此 API：

```js
const controller = new AbortController()
const signal = controller.signal

fetch('./file.json', { signal })
```

您可以设置在 fetch 请求启动 5 秒后触发中止事件的超时，以取消该请求：

```js
setTimeout(() => controller.abort(), 5 * 1000)
```

如果 fetch 已经返回，调用 `abort()` 不会导致任何错误。

当出现中止信号时，fetch 将使用名为 `AbortError` 的 `DOMException` 拒绝 promise：

```js
fetch('./file.json', { signal })
  .then((response) => response.text())
  .then((text) => console.log(text))
  .catch((err) => {
    if (err.name === 'AbortError') {
      console.error('Fetch aborted')
    } else {
      console.error('Another error', err)
    }
  })
```

我在另一篇文章分享了 XHR、Axios 以及今天的主题 Fetch API 如何取消网络请求，如果您想了解，可以查看[取消已发送的网络请求](https://github.com/lio-zero/blog/blob/main/JavaScript/%E5%8F%96%E6%B6%88%E5%B7%B2%E5%8F%91%E9%80%81%E7%9A%84%E7%BD%91%E7%BB%9C%E8%AF%B7%E6%B1%82.md)。

到这里，您已经基本了解了 Fetch API 如何使用。您可以在 👇 下面查看更多关于 Fetch API 的详细内容。

## 更多资料

- [Fetch API 教程](http://www.ruanyifeng.com/blog/2020/12/fetch-tutorial.html)
- [Fetch](https://zh.javascript.info/fetch)
- [Fetch API](https://zh.javascript.info/fetch-api)
- [Fetch：下载进度](https://zh.javascript.info/fetch-progress)
- [Fetch：中止（Abort）](https://zh.javascript.info/fetch-abort)
- [Fetch：跨源请求](https://zh.javascript.info/fetch-crossorigin)
