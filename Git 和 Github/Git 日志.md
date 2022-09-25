# Git 日志记录

查看日志，是我们经常需要使用到的操作，本文将带你了解的一些日志操作。

[`git log`](https://git-scm.com/docs/git-log) 显示当前分支的提交日志。

```bash
git log
```

显示 `commit` 历史，以及每次 `commit` 发生变更的文件

```bash
git log --stat
```

## 保持日志列表简洁

`--oneline` 选项可以保持日志列表简洁。

```bash
git log --oneline
# or
git log --pretty=oneline --abbrev-commit
```

## 指定提交数

指定最近几个提交可以带上 `-*`（`*` 表示数字）

```bash
git log --oneline -5
```

## 显示特定分支的提交

```bash
git log master             # branch
git log origin/master      # branch, remote
git log v1.0.0             # tag
git log master develop     # 多个分支 master 和  develop
```

## 获取漂亮的日志

[`--pretty=format`](https://git-scm.com/docs/git-log#Documentation/git-log.txt---prettyltformatgt) 选项以给定格式漂亮地打印提交日志的内容：

```bash
git log --pretty=format:"%h - %an, %ar : %s"
```

为输出日志设置颜色：

```bash
git log --pretty=format:"`%Cred`%h`%Creset` - %Cgreen(%an, %ar)%Creset : %s"
```

- `%Cred` 将颜色切换为红色
- `%Cgreen` 将颜色切换为绿色
- `%Cblue` 将颜色切换为蓝色
- `%Creset` 重置颜色

详细查阅[文档](https://git-scm.com/docs/git-log#Documentation/git-log.txt-emCredem)。

## 显示一个漂亮的提交历史图表

`--graph` 选项可以以**图形**方式展示日志：

```bash
git log --graph
git log --pretty=format:"%h %s" --graph
```

## 基于时间的日志记录

您可以在特定时间范围内记录条目。非常适合检查每日项目的提交记录。

```bash
git log --since="yesterday" --oneline
git log --since="last month" --oneline
```

如果需要找到给定日期范围之间的提交。可以使用 `--after` 和 `--before` 选项：

```bash
git log --after="2022-6-30" --before="2022-7-1" --oneline
```

## git reflog

另外的一个常用的 [`git reflog`](https://git-scm.com/docs/git-reflog) 命令，它管理 reflog 信息，常用来恢复本地错误操作很重要的一个命令。

```bash
git reflog
```

即使执行 `git reset` 命令之后，您也可以使用 `git reflog` 命令找回那些丢弃掉的提交。
