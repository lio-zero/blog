# 使用 HTML5 download 属性创建可下载的链接

HTML5 引入了许多我们日常开发经常用到的一些方便的特性。其中之一便是在 `<a>` 标签上的 `download` 属性。

默认情况下，锚定标签 `<a>` 的默认值是导航链接，它将转到你在 `href` 属性中指定的链接。

当你添加 `download` 属性时，它表示浏览器应下载锚定指向的资源，而不是导航到该资源。并且你可以向 `download` 属性传递一个字符串值作为可下载文件的名称：

```html
<a href="/logo.png" download>download logo</a>
<!-- 下载的文件名为 logo -->
<a href="/logo.png" download="My Logo.png">download logo</a>
```

> **注意**：IE 11 不支持 `download` 属性。虽然不支持，但也有其专门的 [`polyfill`](https://www.npmjs.com/package/dwnld-attr-polyfill) 来支持在这些不支持的环境中运行该属性。

## 下载限制

值得注意的是，只有当文件与当前网站属于同一个域时，`download` 属性才起作用。如果 `href` 属性与站点的来源不同，则该属性无效。

这是因为资源的请求受浏览器的同源策略所限制，详细内容可以阅读[浏览器同源策略](https://github.com/lio-zero/blog/blob/main/%E6%B5%8F%E8%A7%88%E5%99%A8/%E6%B5%8F%E8%A7%88%E5%99%A8%E5%90%8C%E6%BA%90%E7%AD%96%E7%95%A5.md)。

> 更多：[MDN 给出 `download` 的注意事项](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/a#attr-download)
