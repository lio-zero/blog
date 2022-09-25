# Git Amend

`commit --amend` 用于修改最近的 `commit`。

它将**暂存区（staging environment）**中的更改与最新 `commit` 相结合，并创建一个新的 `commit`。

这个新的 `commit` 完全取代之前的 `commit`。

## Git 修改提交信息

使用 `--amend` 可以做的最简单的事情之一就是更改 `commit` 消息。

假设现在我们修改了 `README.md` 的内容，并且已经 `commit`。

```bash
git add README.md
git commit -m "Adding lines to readme"
```

查找日志：

```bash
git log --oneline
```

发现存在一个拼写错误。我们可以使用 `amend` 提交一个新的 `commit` 去替换它：

```bash
git commit --amend -m "Added lines to README.md"
```

重新查看日志：

```bash
git log --oneline
```

您会发现它已经被替换了。

> **注意**：弄乱 `commit` 存储库的历史可能很危险。通常可以对您自己的本地存储库进行此类更改。但是，您应该避免将历史重写到远程存储库的更改，尤其是在其他人正在使用它们的情况下。

## Git 修改文件

添加文件 `--amend` 的方式与上述相同。只需在提交之前将它们添加到暂存区。
