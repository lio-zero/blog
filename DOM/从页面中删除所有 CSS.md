# 从页面中删除所有 CSS

我想看看没有 CSS 的页面是什么样子的。

我们可以在 DevTools 的 Sources 面板看到 CSS 文件列表，将其删除即可。

但有很多网站将 CSS 内嵌在 `style` 标签中。如果要将其删除，我们需要额外的 JS 操作，可以打开控制台并运行以下命令：

```css
document.querySelectorAll('style,link[rel="stylesheet"]').forEach(item => item.remove())
```

这将删除内联和链接的 CSS。
