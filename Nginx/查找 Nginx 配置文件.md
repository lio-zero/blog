# 查找 Nginx 配置文件

`nginx -t` 和 `nginx -V` 都会打印出默认的 nginx 配置文件路径。

```bash
$ nginx -t
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful

$ nginx -V
nginx version: nginx/1.18.0 (Ubuntu)
built with OpenSSL 1.1.1f  31 Mar 2020
TLS SNI support enabled
configure arguments: --prefix=/etc/nginx --sbin-path=/usr/sbin/nginx --modules-path=/usr/lib/nginx/modules --conf-path=/etc/nginx/nginx.conf ...
```

如果需要，可以通过以下方式获取配置文件：

```bash
$ nginx -V 2>&1 | grep -o '\-\-conf-path=\(.*conf\)' | cut -d '=' -f2
/etc/nginx/nginx.conf
```

即使您加载了其他配置文件，它们仍会打印出默认值。

`ps aux` 将显示当前加载的 nginx 配置文件。

```bash
$ ps aux
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root        11  0.0  0.2  31720  2212 ?        Ss   Jul23   0:00 nginx: master process nginx -c /app/nginx.conf
```

因此，您实际上可以通过以下方式获取配置文件，例如：

```bash
$ ps aux | grep "[c]onf" | awk '{print $(NF)}'
/app/nginx.conf
```
