# Nginx 静态文件与 root 和 alias 混淆

`root` 和 `alias` 指令之间有一个非常重要的区别。这种差异存在于 `root` 或 `alias` 中指定的路径的处理方式中。

`root`：

- `location` 部分会附加到 `root` 部分
- 最终路径为 `root`+ `location`

`alias`：

- `location` 部分将被替换为 `alias` 部分
- 最终路径为 `alias`

假设我们有配置：

```nginx
location /static/ {
  root /var/www/app/static/;
  autoindex off;
}
```

在这种情况下，Nginx 将派生的最终路径是

```txt
/var/www/app/static/static
```

这将返回 `404`，因为 `static/` 中没有 `static/`

这是因为 `location` 部分附加到 `root` 中指定的路径。因此，对于 `root`，正确的方法是

```nginx
location /static/ {
  root /var/www/app/;
  autoindex off;
}
```

另一方面，使用 `alias` 时，`location` 部分会被丢弃。所以对于配置：

```nginx
location /static/ {
  alias /var/www/app/static/; # 注意，这里的斜杆必不可少，但不会包含在最终生成的路径中
  autoindex off;
}
```

最终路径将正确形成为：

```txt
/var/www/app/static
```

在某种程度上，这是有道理的。`alias` 只是让您定义一个新路径来表示现有的**真实**路径。`location` 部分是新路径，因此它被替换为真实路径。将其视为一个符号链接。

另一方面，`root` 并不是一个新路径，它包含一些信息，必须与其他信息进行比较才能生成最终路径。因此，`location` 部分被使用，而不是被丢弃。
