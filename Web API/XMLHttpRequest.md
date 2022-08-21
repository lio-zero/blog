# XMLHttpRequest

首先，我们先来了解**什么是 AJAX**？

[**AJAX**](https://zh.wikipedia.org/wiki/AJAX)（Asynchronous JavaScript and XML，异步 JavaScript 与 XML），指的是一套综合了多项技术的浏览器端网页开发技术。用于创建快速动态网页的技术。

传统的网页（不使用 AJAX）如果需要更新内容，必需重载整个网页。如 `Form` 表单提交，用户点击提交按钮，浏览器将会刷新页面。而通过在后台与服务器进行少量数据交换，AJAX 可以使网页实现异步更新。这意味着可以**在不重新加载整个网页的情况下，对网页的某部分进行更新。**

而 AJAX 的实现，正是基于本文将要讲的主题 `XMLHttpRequest`。

## XMLHttpRequest 介绍

[XMLHttpRequest](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest)（XHR）是微软在九十年代发明的，在 2002-2006 期间，随着所有浏览器的实现，XHR 成为了标准。2006 年，W3C 标准化了 XMLHttpRequest。

在这几年的发展中，像 GMail 或 Google 地图，它们在很大程度上都基于 XHR。

且为了方便使用和跨浏览器 XHR 存在的一些不一致性，像 jQuery 这样的库通过为开发人员提供易于使用的抽象而得到了普及，这反过来又帮助推广了这种技术的使用。

## XHR 请求示例

以下代码创建一个 XMLHttpRequest (XHR) 请求对象，并附加一个响应 `onreadystatechange` 事件的回调函数。

设置 xhr 连接是为了执行对 `https://jsonplaceholder.typicode.com/todos/1` 的 GET 请求，并使用 `send()` 方法发送：

```js
const xhr = new XMLHttpRequest()
xhr.onreadystatechange = () => {
  if (xhr.readyState === 4) {
    xhr.status === 200 ? console.log(xhr.responseText) : console.error('error')
  }
}
xhr.open('GET', 'https://jsonplaceholder.typicode.com/todos/1')
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

状态为：

- 1（OPEN）：请求开始
- 2 (HEADERS_RECEIVED)：HTTP response header 已被接收
- 3（LOADING）：响应正在被加载
- 4 (DONE)：请求完成

## 中止 XHR 请求

可以通过调用 XHR 对象上的 `abort()` 方法中止 XHR 请求。

```js
const xhr = new XMLHttpRequest()

xhr.open('GET', 'https://jsonplaceholder.typicode.com/todos/1', true)
xhr.send()

// 取消发送请求
xhr.abort()
```

如果您对 axios 和 fetch 如何取消已发送的网络请求感兴趣，可以阅读[取消已发送的网络请求](https://github.com/lio-zero/blog/blob/main/JavaScript/%E5%8F%96%E6%B6%88%E5%B7%B2%E5%8F%91%E9%80%81%E7%9A%84%E7%BD%91%E7%BB%9C%E8%AF%B7%E6%B1%82.md)。

## 与 jQuery 的比较

使用 jQuery，这些行可以转换为：

```js
$.get('https://jsonplaceholder.typicode.com/todos/1', (data) => {
  console.log(data)
}).fail((err) => {
  console.error(err)
})
```

## 与 Fetch 的比较

使用 Fetch API：

```js
fetch('https://jsonplaceholder.typicode.com/todos/1')
  .then((res) => res.json())
  .then((data) => console.log(data))
  .catch((err) => {
    console.error(err)
  })
```

使用 `async/await`：

```js
const data = fetch('https://jsonplaceholder.typicode.com/todos/1').then((res) =>
  res.json()
)
const result = await data()
console.log(result)
```

它们都是与 XHR 请求等效的代码。

## 跨域请求

请注意，XMLHttpRequest 连接受到出于安全原因而强制执行的特定限制的约束。

其中最明显的一项是[同源策略](https://github.com/lio-zero/blog/blob/main/%E6%B5%8F%E8%A7%88%E5%99%A8/%E6%B5%8F%E8%A7%88%E5%99%A8%E5%90%8C%E6%BA%90%E7%AD%96%E7%95%A5.md)的执行。

您无法访问另一台服务器上的资源，除非该服务器使用 CORS（跨源资源共享）明确支持这一点。

## 最后

在本文，我们简单的了解了它的一些基本操作。

`XMLHttpRequest` 还有很多其他的内容，您可以在下面提供的文章中了解它们。

## 进一步阅读

- [XMLHttpRequest](https://zh.javascript.info/xmlhttprequest)
- [XMLHttpRequest Level 2 使用指南](https://www.ruanyifeng.com/blog/2012/09/xmlhttprequest_level_2.html)
- [实现一个 AJAX HTTP 请求库](https://github.com/lio-zero/blog/blob/main/%E6%89%8B%E5%86%99%E7%B3%BB%E5%88%97/%E5%AE%9E%E7%8E%B0%E4%B8%80%E4%B8%AA%20Ajax%20HTTP%20%E8%AF%B7%E6%B1%82%E5%BA%93.md)
