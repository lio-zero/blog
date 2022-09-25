# Git Cherry Pick

Git 中的 [Cherry Pick](http://git-scm.com/docs/git-cherry-pick) 意味着从一个分支中选择一个提交，并将其应用到另一个分支上。

这与其他方式形成了对比，例如 `merge` 和 `rebase`，它们通常将许多提交应用于另一个分支。

## 用法

- 确保您位于要将提交应用到的分支上。

```bash
git switch master
# or
git checkout master
```

执行以下操作：

```bash
git cherry-pick <commit-hash>
```

以上命令将指定的提交 `commit-hash`，应用于当前分支。这会在当前分支产生一个新的提交，当然它们的哈希值会不一样。

假设您有 `master` 和 `develop` 分支，现在需要将 `develop` 分支的某一个提交，如 `x` 应用到 `master` 分支，步骤如下：

```bash
git checkout master
git cherry-pick x
```

- 切换到对应分支（这里是 `master`）
- `cherry-pick` 对应的提交（这里是 `x`）

这将在 `master` 分支的末尾增加了一个提交 `x`。

应用 `develop` 分支的最近一次提交，可以直接使用分支名：

```bash
git cherry-pick develop
```

转移多个提交：

```bash
git cherry-pick <HashA> <HashB>
```

## 注意

- 如果您从公共分支中挑选，您应该考虑使用：

```bash
git cherry-pick -x <commit-hash>
```

这将生成一个标准化的提交消息。这样，您（和您的同事）仍然可以跟踪提交的来源，并且可以避免将来发生合并冲突。

- 如果您在提交中附加了注释，则它们不会遵循 `cherry-pick`。要将它们也带过来，您必须使用：

```bash
git notes copy <from> <to>
```

至于其他的情况，例如：配置项、代码冲突和转移到另一个代码库，

推荐阅读阮一峰老师的 [git cherry-pick 教程](https://www.ruanyifeng.com/blog/2020/04/git-cherry-pick.html)，其中介绍了其他的示例，例如：配置项、代码冲突和转移到另一个代码库。

## 进一步阅读

- [Git Cherry Pick](https://www.atlassian.com/git/tutorials/cherry-pick)
- [How to cherry-pick a range of commits and merge them into another branch?](https://stackoverflow.com/questions/1994463/how-to-cherry-pick-a-range-of-commits-and-merge-them-into-another-branch)
- [What does cherry-picking a commit with Git mean?](https://stackoverflow.com/questions/9339429/what-does-cherry-picking-a-commit-with-git-mean)
- [How to git-cherry-pick only changes to certain files?](https://stackoverflow.com/questions/5717026/how-to-git-cherry-pick-only-changes-to-certain-files)
- [Is it possible to cherry-pick a commit from another git repository?](https://stackoverflow.com/questions/5120038/is-it-possible-to-cherry-pick-a-commit-from-another-git-repository)
- [How to cherry-pick multiple commits](https://stackoverflow.com/questions/1670970/how-to-cherry-pick-multiple-commits)
