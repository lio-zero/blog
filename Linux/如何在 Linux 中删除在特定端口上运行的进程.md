# 如何在 Linux 中删除在特定端口上运行的进程

以 80 端口为例。

首先，列出任何监听端口 80 的进程：

```bash
# 使用 lsof
$ lsof -i:80

# 使用 fuser
$ fuser 80/tcp
```

使用 `kill` 命令删除任何监听端口 80 的进程：

```bash
kill $(lsof -t -i:80)
kill -9 $(lsof -t -i:80)
```

或使用 `fuser` 命令：

```bash
fuser -k 80/tcp
```

## 更多资源

- [Finding the PID of the process using a specific port?](https://unix.stackexchange.com/questions/106561/finding-the-pid-of-the-process-using-a-specific-port)
- [How to kill a process running on particular port in Linux?](https://stackoverflow.com/questions/11583562/how-to-kill-a-process-running-on-particular-port-in-linux)
