# git pull 和 git fetch 的区别

来自 Git 文档 [`git pull`](http://git-scm.com/docs/git-pull)：

> 在默认模式下，`git pull` 是 `git fetch` 后跟的简写 `git merge FETCH_HEAD`。

- 当你 `pull` 时，Git 会尝试自动合并。它是上下文敏感的，因此 Git 会将任何拉入的提交合并到您当前工作的分支中。`pull` 自动合并提交，而不让您先查看它们。如果你不小心管理你的分支，你可能会经常遇到冲突。
- 当你 `fetch` 时，Git 从目标分支收集当前分支中不存在的任何提交，并将其存储在本地存储库中。但是，它不会将它们与当前分支合并。如果您需要使存储库保持最新状态，但正在处理更新文件可能会导致冲突时，这一点尤其有用。要将提交集成到当前分支中，必须在之后使用 `merge`。

当你 `fetch` 时，你可以先使用 `diff` 查看更新后的差异，在选择 `merge`。

## 更多资料

[What is the difference between 'git pull' and 'git fetch'?](https://stackoverflow.com/questions/292357/what-is-the-difference-between-git-pull-and-git-fetch)
