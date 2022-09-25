# 删除本地和远程 Git 分支

## 删除本地分支

要删除本地分支，可以使用以下方法之一：

```bash
git branch -d <branch_name>
git branch -D <branch_name>
```

- `-d` 选项是 `--delete` 的别名，它只在分支已经完全合并到其上游分支时删除该分支。如果您尝试删除当前选定的分支，您将收到错误消息。
- `-D` 选项是 `--delete --force` 的别名，它删除“无论其合并状态如何”的分支。（强制删除）

## 删除远程分支

Git v1.5.0 删除远程分支使用：

```bash
git push <remote_name> :<branch_name>
```

Git v1.7.0 删除远程分支使用：

```bash
git push <remote_name> --delete <branch_name>
```

从 Git v2.8.0 开始，我们可以使用 `-d` 选项作为 `--delete` 的别名。使用这些语法取决于你使用的 Git 版本。

示例：

```bash
git push origin :next
```

上面命令表示删除 `origin` 主机的 `next` 分支。

## 删除本地远程跟踪分支

如果需要删除远程上不再存在的任何远程分支的所有过时的本地远程跟踪分支，可以使用：

```bash
$ git branch --delete --remotes <remote_name>/<branch_name>
$ git branch -dr <remote_name>/<branch_name>

# 从 Git v1.6.6 开始
$ git fetch <remote_name> --prune # 删除多个过时的远程跟踪分支
$ git fetch <remote_name> -p      # 简写
```

> **注意**：在大多数情况下，`<remote_name>` 将是 `origin`

## 更多资料

[How do I delete a Git branch locally and remotely?](https://stackoverflow.com/questions/2003505/how-do-i-delete-a-git-branch-locally-and-remotely)
