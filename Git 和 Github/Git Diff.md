# Git Diff

当我们执行 `git fetch` 拉取远程最新代码到本地，但想要在 `git merge` 合并之前查看一些文件的变动时，可以使用 `git diff` 命令。

> 如果是少量代码的改动，`git diff` 还是挺方便的，但如果代码量较大，建议使用 GUI 工具。

本文整理了一些常用的命令。

## 查看变化中的差异

`git diff` 显示暂存区和工作区的差异：

```bash
# 所有文件的差异
git diff

# 具体某个文件的差异
git diff [file]
```

## 显示暂存区与上次 commit 的差异

`--cached` 和 `--staged` 都可以用于显示暂存区与上次 commit 的差异：

```bash
git diff --cached [file]
git diff --staged [file]
```

## 查看两个分支之间的差异

使用 `git diff <branch>...<other-branch>` 查看 `<branch>` 和 `<other-branch>` 之间的差异 。

- `git diff <branch>...<other-branch>` 比较两个分支。
- `git diff 76ce3e6...e251d8d` 比较两个特定的提交。

```bash
$ git diff <branch>...<other-branch>
$ git diff patch-1...patch-2
# 显示分支 patch-1 和 patch-2 之间的差异
```

## 统计数据

`--stat` 生成差异统计。

```bash
$ git diff --stat

# app/a.txt    | 2 +-
# app/b.txt    | 8 ++----
# 2 files changed, 10 insertions(+), 84 deletions(-)
```

`--shortstat` 是 `--stat` 选项的简化版本，它不显示具体的文件名及一些图形部分，只输出最后一行统计数据。

显示今天写了多少行代码：

```bash
git diff --shortstat "@{0 day ago}"
# 10 files changed, 62 insertions(+), 15 deletions(-)
```

## 仅显示修改过的文件名

```bash
git diff --summary
```
