# Git 常用配置和技巧

以下使用 `--global` 标志全局配置的命令可以 `~/.gitconfig` 文件下查看。

## 创建别名

使用 `<alias>` 可以极大地提高效率，我常用的有

使用下面的命令创建别名，将 `<alias>` 替换为别名名称，将 `<command>` 替换为要使用别名的命令：

```bash
git config --global alias.<alias> <command>
```

常用的别名有：

```bash
$ git config --global alias.co checkout
$ git config --global alias.br branch

# 使用你喜欢的或自己的
[alias]
  co = checkout
  cob = checkout -b
  coo = !git fetch && git checkout
  br = branch
  brd = branch -d
  st = status
  aa = add -A .
  unstage = reset --soft HEAD^
  cm = commit -m
  amend = commit --amend -m
  fix = commit --fixup
  undo = reset HEAD~1
  rv = revert
  cp = cherry-pick
  pu = !git push origin `git branch --show-current`
  fush = push -f
  mg = merge --no-ff
  rb = rebase
  rbc = rebase --continue
  rba = rebase --abort
  rbs = rebase --skip
  rom = !git fetch && git rebase -i origin/master --autosquash
  save = stash push
  pop = stash pop
  apply = stash apply
  rl = reflog
  last = log -1 HEAD
  logl = log --oneline
```

接下来，试下效果：

```bash
git st
git cm "xxx"
```

## 强制推送

有时，您需要将更改推送到远程存储库并覆盖文件。您可能习惯于使用 `git push --force`。

但是如果其他人已经将更改推送到同一分支，会发生什么？您的命令将清除他们的提交。**这肯定是不行的**。

Git 有一种更安全的方式来推送更改并覆盖您的提交。而不是使用的 `--force` 标志，你可以使用 `--force-with-lease`。此标志将防止您意外覆盖其他人的提交。这种工作方式有点神奇，但您可以相信它确实如此。

您可以 `force-push` 为此编写别名：

```bash
git config --global alias.force-push "push --force-with-lease"
```

现在您可以运行 `git force-push`，这更容易记住。当然，为了节省几次按键操作，您还可以添加此别名的简短版本。

```bash
git config --global alias.fp force-push
```

请注意，您只能在 Git 2.20+ 中为别名设置别名。

## 设置默认推送分支名称

默认情况下，推送时使用当前分支的名称作为远程分支的名称。

- 使用 `git config push.default current` 将远程分支的名称设置为当前本地分支的名称作为默认名称。
- 可以使用 `--global` 标志全局配置此选项。

```bash
$ git config [--global] push.default current
$ git config --global push.default current

$ git checkout -b my-branch
$ git push -u
# 推送 origin/my-branch
```

## 默认情况下禁用快进合并

- 使用 `git config --add merge.ff false` 禁用所有分支的快进合并，即使可能。
- 您可以使用 `--global` 标志全局配置此选项。

```bash
$ git config [--global] --add merge.ff false
$ git config --global --add merge.ff false

$ git checkout master
$ git merge my-branch
# 即使有可能也不会快进
```

## 列出所有 Git 别名

- 使用 `git config -l` 列出的配置文件中设置的所有变量。
- 使用管道运算符（`|`）管道输出，`grep alias` 表示仅保留别名。
- 使用管道运算符（`|`）管道传输，并使用 `sed` 的 `/^alias\.//g'` 删除别名。 每个别名的一部分。

```bash
$ git config -l | grep alias | sed 's/^alias\.//g'
# st=status
# co=checkout
# rb=rebase
```

## 添加提交消息模板

为当前存储库设置提交消息模板。可以使用 `git config commit.template` 指定 `<file>` 的提交当前库信息模板。

```bash
git config commit.template <file>
```

假设我们使用 `"commit-template"`作为我们的提交消息模板：

```bash
git config commit.template "commit-template"
```

`commit-template` 参考如下：

```bash
fix(<模块>): <描述>

#<具体描述>

#<问题单号>

# type 字段包含:
# feat：新功能（feature）
# fix：修补bug
# docs：文档（documentation）
# style： 格式（不影响代码运行的变动）
# refactor：重构（即不是新增功能，也不是修改bug的代码变动）
# test：增加测试
# chore：构建过程或辅助工具的变动
# scope：用于说明 commit 影响的范围，比如数据层、控制层、视图层等等。
# subject：是 commit 目的的简短描述，不超过50个字符
# Body：部分是对本次 commit 的详细描述，可以分成多行
# Footer：用来关闭 Issue或以BREAKING CHANGE开头，后面是对变动的描述、以及变动理由和迁移方法
```

详细的 Git 提交规范，可以参考 [Vue](https://github.com/vuejs/vue/blob/dev/.github/COMMIT_CONVENTION.md) 或 [Angular](https://github.com/conventional-changelog/conventional-changelog/tree/master/packages/conventional-changelog-angular) 规范。

## 配置 Git 用户信息

- 使用 `git config user.email <email>` 为当前储存库设置用户的电子邮件。
- 使用`git config user.name <name>` 为当前储存库设置的用户名。
- 您可以使用 `--global` 标志来配置全局用户信息。

```bash
git config [--global] user.email <email>
git config [--global] user.name <name>
```

配置当前存储库的用户

```bash
git config user.email "xxx@xxx.xxx"
git config user.name "O.O"
```

配置全局 Git 用户

```bash
git config --global user.email "xxx@xxx.xxx"
git config --global user.name "O.O"
```

## 配置存储库的行尾

- 使用 `git config core.eol [lf | crlf]` 配置行尾。
- `lf` 是 UNIX 行的结尾（`\n`），而 `crlf` DOS 行的结尾是（`\r\n`）。

```bash
git config core.eol [lf | crlf]
```

配置为使用 UNIX 行结束符

```bash
git config core.eol lf
```

## 自动更正 Git 命令

将 Git 配置为自动更正键入错误的命令。可以使用 `git config --global help.autocorrect 1` 使 Git 的自动更正。

```bash
git config --global help.autocorrect 1
git sttaus # 改为运行 "git status"
```

## 配置 Git 文本编辑器

默认情况下打开 Git 使用的是 `vi` 编辑器，如果你没怎么使用过 Linux 系统的话，可能不是很熟。你可以通过下面的命令来使用其他编辑器打开 Git。

使用 `git config --global core.editor <editor-command>` 调用 `<editor-command>` 作为 Git 文本编辑器。

```bash
git config --global core.editor <editor-command>
```

将 VS Code 设置为 Git 文本编辑器

```bash
git config --global core.editor "code --wait"
```

将 `vi` 作为 Git 的文本编辑器

```bash
git config --global core.editor "vi"
```

## 编辑 Git 配置文件

使用 `git config --global -e` 在默认 Git 文本编辑器中打开 Git 全局配置文件。

```bash
git config --global -e
```

## shell 别名

```bash
git config --global alias.hello "!echo hello"
```

```bash
git config --global alias.dad '!curl https://icanhazdadjoke.com/ && echo'
```
