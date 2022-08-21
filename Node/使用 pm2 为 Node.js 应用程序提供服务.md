# 使用 pm2 为 Node.js 应用程序提供服务

[PM2](https://pm2.keymetrics.io/) 是 node 进程管理工具，可以利用它来简化很多 node 应用管理的繁琐任务，如性能监控、自动重启、负载均衡等，而且使用非常简单。

## PM2 特性

- 内建负载均衡（使用 Node cluster 集群模块）
- 后台运行
- 0 秒停机重载
- 具有 Ubuntu 和 CentOS 的启动脚本
- 停止不稳定的进程（避免无限循环）
- 控制台检测
- 提供 HTTP API
- 远程控制和实时的接口 API（Node.js 模块允许和 PM2 进程管理器交互）

## 全局安装

```bash
npm i -g pm2
```

## 常用命令

pm2 的功能十分强大，除了重启进程以外，还能实时收集日志和监控。

### 分叉模式（Fork mode）

启动程序，并为进程提供一个名称：

```bash
pm2 start app.js --name my-app
```

### 集群模式（Cluster mode）

```bash
# 指定同时起多少个进程（进程数由 CPU 核数决定），组成一个集群
$ pm2 start app.js -i max
$ pm2 start app.js -i 0 # 将根据可用的 CPU 使用 LB 启动最大进程
$ pm2 start app.js -i 4 # 启动4个应用程序实例，并将网络请求负载均衡到每个应用中。


$ pm2 start app.js -o out.log  # 标准输出日志文件的路径
$ pm2 start app.js -e err.log  # 错误输出日志文件的路径
$ pm2 start app.js -e err.log -o out.log

# 用 fork 模式启动而非 cluster
$ pm2 start app.js -x

# 将应用程序调整到10个实例
$ pm2 scale [app-name] 10
```

### 进程监控

```bash
# 列出所有正在运行的进程
$ pm2 list
$ pm2 ls
```

![pm2-list.png](https://upload-images.jianshu.io/upload_images/18281896-7323a06d7f74660e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```bash
# 原始 JSON 中的打印进程列表
$ pm2 jlist

# 美化 JSON 中的打印进程列表
$ pm2 prettylist

# 显示有关特定进程的所有信息
$ pm2 describe 0

# 显示指定进程的所有信息
$ pm2 show APP-NAME

# 列出所有 node 进程的 CPU 和内存统计数据，如：监视日志、自定义指标和进程信息
$ pm2 monit
```

![pm2-monit.png](https://upload-images.jianshu.io/upload_images/18281896-7b37b90a767f2a6e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 日志管理

```bash
$ pm2 logs # 查看所有日志
$ pm2 logs APP-NAME  # 显示 APP-NAME 日志
$ pm2 logs --json    # JSON 输出
$ pm2 logs --format  # JSON 格式化输出

# 清空所有日志文件（刷新所有日志）
$ pm2 flush

# 重新加载所有日志
$ pm2 reloadLogs
```

### 进程状态管理（重启、停止、删除）

```bash
# 可启动 Node、Python、bash 脚本、JSON 文件等。
$ pm2 start app.js

# 如果 pm2 守护进程还不存在，就在前台运行它
$ pm2 start app.js --no-daemon

# 跳过 vizion 功能（版本控制）
$ pm2 start app.js --no-vizion

# 不自动重新启动 app
$ pm2 start app.js --no-autorestart

# 监听应用目录的变化，发生变化时，自动重启。
$ pm2 start app.js --watch

# 排除监听的目录/文件，可以是特定的文件名，也可以是正则。比如 --ignore-watch="test node_modules "some scripts""
$ pm2 start app.js --ignore-watch

# 应用的名称。查看应用信息的时候可以用到。
$ pm2 start app.js -n --name

# 停止特定进程 ID
$ pm2 stop 0
$ pm2 stop app.js

# 停止所有进程
$ pm2 stop all

# 重新启动特定进程 ID
$ pm2 restart 0

# 重新启动所有进程
$ pm2 restart all

# 将从 pm2 列表中删除特定进程
$ pm2 delete 0

# 将从 pm2 列表中删除所有进程
$ pm2 delete all

# 0s 停机重新加载进程（对于 NETWORKED 进程）
# 这项功能允许你重新载入代码而不用失去请求连接。
# Note：仅能用于 web 应用，运行于 Node 0.11.x 版本，运行于 cluster 模式（默认模式）
$ pm2 reload all

# 清除所有进程
$ pm2 kill

# 将在 .pm2 文件夹下生成默认文件名为 dump.pm2 文件，导出 pm2 list 中的所有项目启动方式的数据
$ pm2 dump

# 重新导出新数据，覆盖 dump.pm2 文件
$ pm2 resurect

# 保存进程列表，可在重新启动时恢复
$ pm2 save
```

### 开机自动启动

要为 PM2 生成启动脚本，可以运行以下命令：

```bash
pm2 startup
```

确保在启动时所有 PM2 进程都在运行，可以运行以下命令：

```bash
pm2 save
```

这将创建文件 `dump.pm2` 文件，它将自动启动我们的脚本。

也就是说，每当我们的服务器重启时，进程都会自动重启。

如果您需要手动重启所有进程，可以执行以下命令。

```bash
pm2 resurrect
```

> Tips：如果按上面步骤遇到错误。可以看看这篇 **[在 windows 上安装 pm2 并设置开机启动](https://www.huaface.com/article/11)**。

### 其他

```bash
# 启动 web 界面 http://localhost:9615
$ pm2 web

# 重置元数据（重新启动时间。。。）
$ pm2 reset

# 内存 pm2 更新
$ pm2 updatePM2

# 确保 pm2 守护进程已启动
$ pm2 ping

# 向脚本发送系统信号
$ pm2 sendSignal SIGHUP my-app
```

## 更多资料

[Right Sizing PM2 Clusters](https://vadosware.io/post/right-sizing-pm2-clusters/)
