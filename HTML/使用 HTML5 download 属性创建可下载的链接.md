# 使用 HTML5 download 属性创建可下载的链接

HTML5 引入了许多我们日常开发经常用到的一些方便的特性。其中之一便是在 `<a>` 标签上的 `download` 属性。

默认情况下，锚定标签 `<a>` 的默认值是导航链接，它将转到您在 `href` 属性中指定的链接。

当您添加 `download` 属性时，它表示浏览器应下载锚定指向的资源，而不是导航到该资源。并且你可以向 `download` 属性传递一个字符串值作为可下载文件的名称：

```html
<a href="/logo.png" download></a>
<!-- 下载的文件名为 'logo' -->
<a href="/logo.png" download="logo">home</a>
```

> **注意**：IE 11 不支持 `download` 属性。虽然不支持，但也有其专门的 [`polyfill`](https://www.npmjs.com/package/dwnld-attr-polyfill) 来支持在这些不支持的环境中运行该属性。

## 下载限制

值得注意的是，只有当文件与当前网站属于同一个域时，`download` 属性才起作用。如果 `href` 属性与站点的来源不同，则该属性无效。

> 更多：[MDN 给出 `download` 的注意事项](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/a#attr-download)

## 什么是同源政策？

> [**同源策略**](https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin_policy)是一个重要的安全策略，它用于限制一个 [origin](https://developer.mozilla.org/zh-CN/docs/Glossary/Origin) 的文档或者它加载的脚本如何能与另一个源的资源进行交互。它能帮助阻隔恶意文档，减少可能被攻击的媒介。

也就是说，用户只能下载来自源站点的文件。下面将给出 URL `http://www.google.com/` 的源进行比对的示例：

| 源：`https://www.google.com/`                | 结果 | 原因                                  |
| -------------------------------------------- | ---- | ------------------------------------- |
| `https://www.google.com/logo.png`            | 同源 | 只有路径不同                          |
| `https://www.google.com/public/img/logo.png` | 同源 | 只有路径不同                          |
| `http://www.google.com/logo.png`             | 失败 | 协议不同                              |
| `https://www.google.com:444/logo.png`        | 失败 | 端口不同 ( `https://` 默认端口是 443) |
| `https://www.google.com/dir/logo.png`        | 失败 | 域名不同                              |
