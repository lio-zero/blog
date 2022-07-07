# curl -fsSL

在浏览 [antfu](https://github.com/antfu) 的 [1990-script](https://github.com/antfu/1990-script) 项目时，发现其中有这样一条命令：

```bash
sh -c "$(curl -fsSL https://raw.github.com/antfu/1990-script/master/index.sh)"
```

其中好奇的是 curl `-fsSL` 参数各自的含义，于是便开始了日常搜索。

- `-f（--fail）` — 表示在服务器错误时，阻止一个返回的表示错误原因的 HTML 页面，而由 curl 命令返回一个错误码 22 来提示错误。
- `-L（--location）` — 参数会让 HTTP 请求跟随服务器的重定向。curl 默认不跟随重定向。
- `-S（--show-error）` — 指定只输出错误信息，通常与 `-s` 一起使用。
- `-s（--silent）` — 不显示错误和进度信息。

我们可能会经常使用这个命令来下载例如 GitHub 中的项目文件。

## 相关资料

- [curl](https://curl.se/) 官网
- 推荐阮一峰老师的 [curl 的用法指南](https://www.ruanyifeng.com/blog/2019/09/curl-reference.html) 和 [curl 网站开发指南](https://www.ruanyifeng.com/blog/2011/09/curl.html)。
