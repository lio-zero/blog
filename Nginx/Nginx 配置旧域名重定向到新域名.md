# Nginx 配置旧域名重定向到新域名

Nginx 里的 `rewrite` 模块是专门负责静态重写的。

该模块允许使用正则表达式改变 URI，并且根据变量来重定向以及选择配置。

基本用法是：`rewrite patten replace flag`。`patten` 是正则表达式，与 `patten` 匹配的 URL 会被改写为 `replace`，而 `flag` 是可选的，可以有如下标志：

- `last` — 完成 `rewrite`，然后搜索相应的 URI 和位置
- `break` — 中止 `rewrite`，不再匹配后面的规则
- `redirect` — 返回 code 为 302 的临时重定向
- `permanent` — 返回 code 为 301 的永久重定向

例如，要将旧域名重定向到新域名上：

```nginx
server {
  listen 443 ssl;
  server_name old.com;
  # ...

  rewrite .* https://new.com;
}
```

如果是跳转到新域名上时要保留 `path` 部分，那么：

```nginx
server {
  listen 443 ssl;
  server_name old.domain.com;
  # ...

  rewrite ^/(.*)$ https://new.domain.com/$1;
}
```

你还可以添加条件判断，如果域名不是 `new.domain.com` 就统一转到 `https://new.domain.com`：

```nginx
server {
  listen 443 ssl;
  server_name old.domain.com new.domain.com example.com www.example.com;

  if ($host != 'new.domain.com') {
    rewrite ^/(.*)$ https://new.domain.com/$1 permanent;
  }
}
```

`$host` 是 `core` 模块内部的一个变量，当请求头里不存在 `host` 属性或者是个空值，`$host` 则等于 `server_name`。如果请求头里有 `host` 属性，那么 `$host` 等于 `host` 属性除了端口号的部分，例如 `host` 属性是 `www.example.com`，那么 `$host` 就是 `www.example.com`。

> **注意**：`permanent` 是永久重定向，如果重定向的地址错误，由于浏览器会记住它，会一直重定向你设置的错误地址。这时你可以通过清除浏览器缓存解决。

`rewrite` 与 `location` 配合实现图片文件跳转到 CDN：

```nginx
location ~ .*\.(gif|jpg|jpeg|png|bmp|swf)$ {
  expires 30d;
  rewrite ^/uploadfile\/(.*)$ https://cdn.new.domain.com/uploadfile/$1;
}
```

[nginx-tutorial](https://dunwu.github.io/nginx-tutorial) 教程推荐的最佳做法是，[使用 `return` 代替 `rewrite` 来做重定向](https://dunwu.github.io/nginx-tutorial/#/nginx-configuration?id=%e4%bd%bf%e7%94%a8-return-%e4%bb%a3%e6%9b%bf-rewrite-%e6%9d%a5%e5%81%9a%e9%87%8d%e5%ae%9a%e5%90%91)。

因为它们比通过位置块评估 RegEx 更简单、更快捷。该指令停止处理，并将指定的代码返回给客户端。

```nginx
server {
  # ...

  return 301 https://new.domain.com$request_uri;
}
```

`$request_uri` 也是一个内置变量，用于替代正则以高效的填补现有 URI。

## 更多资料

[访问后台出现重定向次数过多该怎么办？-建站需知](https://blog.csdn.net/AlvinCasper/article/details/112727903)
