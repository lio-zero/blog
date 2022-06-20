# 结合 Google 字体请求

通常我们在加载不同的 [Google fonts](https://fonts.google.com/) 时添加单独的链接，如下所示：

```html
<link
  href="https://fonts.googleapis.com/css?family=Open+Sans:400,600"
  rel="stylesheet"
/>
<link
  href="https://fonts.googleapis.com/css?family=Roboto:400,700"
  rel="stylesheet"
/>
```

发送到 Google 的 HTTP 请求数量可以根据我们要加载的字体数量增加。它会影响页面的加载时间。

建议使用 `|` 字符将请求合并为单个请求。链接可能如下所示：

```html
<link
  href="https://fonts.googleapis.com/css?family=Open+Sans:400,600|Roboto:400,700"
  rel="stylesheet"
/>
```

请注意，使用 Google Font v2 时，语法略有不同。它允许传递多个 `family` 参数：

```html
<link
  href="https://fonts.googleapis.com/css2?family=Open+Sans:wght@400,600&family=Roboto:wght@400,700"
  rel="stylesheet"
/>
```
