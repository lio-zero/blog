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
  # ...

  server_name old.com;
  rewrite .* https://new.com;
}
```

如果是跳转到新域名上时要保留路径，那么：

```nginx
server {
  listen 443 ssl;
  server_name old.domain.com;
  # ...

  rewrite ^/(.*)$ https://new.domain.com/$1;
}
```

还有一种方式，如果域名不是 `www.new.domain.com` 就统一转到 `https://www.new.domain.com`：

```nginx
server {
  listen 443 ssl;
  server_name old.domain.com new.domain.com example.com www.example.com;
  if ($host != 'www.new.domain.com') {
    rewrite ^/(.*)$ https://new.domain.com/$1 permanent;
  }
}
```

`$host` 是 `core` 模块内部的一个变量，当请求头里不存在 `host` 属性或者是个空值，`$host` 则等于 `server_name`。如果请求头里有 `host` 属性，那么 `$host` 等于 `host` 属性除了端口号的部分，例如 `host` 属性是 `www.example.com`，那么 `$host` 就是 `www.example.com`。

也可以单独增加一个 server，在里面统一设置，`permanent` 是 301 重定向：

```nginx
server {
  listen 443 ssl;
  server_name new.domain.com;
  # ...

  rewrite ^/(.*)$ https://www.new.domain.com/$1 permanent;
}
```

`rewrite` 与 `location` 配合实现图片文件跳转到 CDN：

```nginx
location ~ .*\.(gif|jpg|jpeg|png|bmp|swf)$ {
  expires 30d;
  rewrite ^/uploadfile\/(.*)$ https://cdn.new.domain.com/uploadfile/$1;
}
```

> **注意**：`permanent` 是永久重定向，如果重定向的地址错误，由于浏览器会记住它，会一直重定向你设置的地址。这时你可以通过清楚浏览器缓存解决。

## 更多资料

[访问后台出现重定向次数过多该怎么办？-建站需知](https://blog.csdn.net/AlvinCasper/article/details/112727903)
