# 在 Google 字体请求中加载特定字符

默认情况下，[Google Fonts](https://fonts.google.com/) 加载包含其支持的完整字符集的完整字体。

我们可以要求谷歌下载包含所需字符的部分字体，而不是下载整个字体。

我们可以将字符传递给 `text` 参数：

```html
<link
  href="https://fonts.googleapis.com/css2?family=Sacramento&text=MyHeading"
  rel="stylesheet"
/>
```

如果希望使用 Unicode 字符，那么使用其 UTF-8 表示形式对其进行编码。例如，`tips & tricks` 表示为 `tips+%26+tricks`。

减小字体文件的大小可以缩短加载时间，尤其是在网络速度通常有限的设备上。

## 更多资料

[结合 Google 字体请求](https://github.com/lio-zero/blog/blob/main/HTML/%E7%BB%93%E5%90%88%20Google%20%E5%AD%97%E4%BD%93%E8%AF%B7%E6%B1%82.md)
