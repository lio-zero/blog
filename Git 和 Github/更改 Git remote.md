# 更改 Git remote

假设您需要拷贝当前文件夹作为一个副本，存放在另一个 GitHub repo 中，接着该如何操作？

首先，我们在终端运行：

```bash
git remote -v
```

这将列出现有的 GitHub **origin** 远程存储库。

然后，运行：

```bash
git remote rm origin
# or
git remote remove origin
```

这删除了 origin remote，因此再次运行 `git remote -v` 不再返回任何内容。

现在，你可以重新将本地仓库与远程仓库建立连接：

```bash
git remote add origin <url>
```

> origin 默认是远程仓库别名，url 可以是 https 或者 ssh 的方式建立链接。

你还可以使用 `set-url` 命令在不删除远程 remote 的情况下，直接切换 origin URL：

```bash
git remote set-url origin <url>
```

## 其他常用 remote 命令

其他常用的 [git-remote 命令](https://git-scm.com/docs/git-remote)：

- `git remote show <name>` 显示某个远程仓库的信息
- `git remote rename <old_name> <new_name>` 远程仓库重新命名
- `git remote prune <name>` 删除与 `<name>` 关联的过时引用
