# Git restore

[`git restore`](https://git-scm.com/docs/git-restore) 用于恢复工作树文件。

`git restore` 的作用是将工作区但是不在暂存区的文件撤销更改。

也就是还未使用 `git add` 将文件添加到暂存区。这时，我们可以使用 `git restore [file]` 将文件内容恢复到还未修改之前的状态。

```bash
git restore
```

而添加 `--staged` 选项可以将暂存区的文件撤出，但不会更改文件的内容。

```bash
git restore [file] --staged
```

每个 Git 命令的可选项有很多，详细内容请查阅文档。
