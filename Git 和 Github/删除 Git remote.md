# 删除 Git remote

以一个示例为例，假设您需要拷贝当前文件夹作为一个副本，存放在另一个 GitHub repo 中，该如何操作？

首先，我们在终端运行：

```git
git remote -v
```

这将列出现有的 GitHub **origin**远程存储库。

然后，运行：

```bash
git remote rm origin
# or
git remote remove origin
```

这删除了 origin remote，因此再次运行 `git remote -v` 不再返回任何内容。

现在，你可以重新将本地仓库与远程仓库建立连接：

```git
git remote add origin url
```

> origin 默认是远程仓库别名，url 可以是 https 或者 ssh 的方式建立链接

## 其他常用 remote 命令

其他常用的 [git-remote 命令](https://git-scm.com/docs/git-remote)：

- `git remote show <name>` 显示某个远程仓库的信息
- `git remote rename <old_name> <new_name>` 远程仓库重新命名
- `git remote set-url <name> url` 更改远程存储库的 URL
- `git remote prune <name>` 删除与 `<name>` 关联的过时引用
