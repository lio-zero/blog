# 使用 sendFile() 在 Express 中发送静态文件

Express [`sendFile()` 方法](https://expressjs.com/en/api.html#res.sendFile)允许您发送原始文件作为对 HTTP 请求的响应。您可以将其 `res.sendFile()` 视为单个接口的 Express `static` 中间件。

## 使用 `sendFile()`

假设您有一个如下所示的 HTML 文件 `index.html`：

```html
<h1>Hello World</h1>
```

通过将路径传递到 `index.html`，可以使用 `res.sendFile()` 使 Express 将此 HTML 文件作为 HTTP 响应提供。

> **注意**：除非指定 `root` 选项，否则路径必须是绝对路径。

```js
app.get('/', (req, res) => {
  res.sendFile(`${__dirname}/index.html`)
})
```

如果不想指定绝对路径，可以通过 `root` 选项以指定路径所相对于的目录。

```js
app.get('/', (req, res) => {
  res.sendFile('index.html', { root: __dirname })
})
```
