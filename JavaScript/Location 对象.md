# Location 对象

浏览器公开的 `window.location` 属性指向一个 [Location](https://developer.mozilla.org/en-US/docs/Web/API/Location) 对象，它是 `window` 对象的一部分，包含有关当前 URL 的信息。

属性：

- `location.hash` — 设置或返回从井号（`#`）开始的 URL 的锚部分。
- `location.host` — 设置或返回主机名和端口。
- `location.href` — 设置或返回完整的 URL。
- `location.port` — 返回 web 主机的端口（80 或 443）。
- `location.search` — 设置或返回从问号（`?`）开始的 URL 的查询部分。
- `location.hostname` — 返回 web 主机的域名。
- `location.pathname` — 返回当前页面的路径名。
- `location.protocol` — 返回所使用的 web 协议（`http:` 或 `https:`）。

方法：

- `location.reload()` 重新加载当前文档。
- `location.replace(url)` 以给定的 URL 来替换当前的资源。

```js
// 将跳转到该链接相应的页面
location.replace('https://github.com/lio-zero')
```

- `location.assign(url)` 会触发窗口加载并显示指定的 URL 的内容，加载新的文档。

```js
// 跳转到给定 URL 的页面
location.assign('https://github.com/lio-zero')
```

`replace()` 和 `assign()` 不同在于，调用 `replace()` 方法后，当前页面不会保存到会话历史中。这样，用户点击回退按钮时，将不会再跳转到该页面。（不保存跳转前的页面）

## HTTP 跳转 HTTPS

如果页面当前访问的是 HTTP 协议页面，则将其重定向到 HTTPS。

```js
const httpsRedirect = () => {
  if (location.protocol !== 'https:')
    location.replace('https://' + location.href.split('//')[1])
}

httpsRedirect() // 若在 http://www.baidu.com，则跳转到 https://www.baidu.com
```

## 重定向到指定的 URL

- 使用 `location.href` 或 `location.replace()` 重定向到指定的 URL。
- 传递第二个参数来模拟链接点击（默认）或 HTTP 重定向。

```js
const redirect = (url, asLink = true) =>
  asLink ? (location.href = url) : location.replace(url)

redirect('https://github.com/lio-zero')
```

## 更多资料

[重定向到另一个页面](https://github.com/lio-zero/blog/blob/main/JavaScript/%E9%87%8D%E5%AE%9A%E5%90%91%E5%88%B0%E5%8F%A6%E4%B8%80%E4%B8%AA%E7%BD%91%E9%A1%B5.md)
