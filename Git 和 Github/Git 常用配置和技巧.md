# Git 常用配置和技巧

以下使用 `--global` 标志为全局配置命令，可以在全局的 `~/.gitconfig` 文件下查看。

## 创建别名

[Git Aliases](https://git-scm.com/book/en/v2/Git-Basics-Git-Aliases) 允许我们为 Git 命令设置别名，极大地提高效率。

使用下面的命令创建别名，将 `<alias>` 替换为别名名称，将 `<command>` 替换为要使用别名的命令：

```bash
git config --global alias.<alias> <command>
```

例如：

```bash
git config --global alias.co checkout
git config --global alias.br branch
```

常用的别名有：

```bash
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
  nah = git reset --hard; git clean -df
  gac = git add . && git commit -a -m
  wip = git add . && git commit -a -m 'WIP'
```

如果别名很多，你不必一条条设置，可以打开 `~/.gitconfig` 文件，将别名粘贴到 `[alias]` 下即可。

接下来，试下效果：

```bash
git st
git cm "chore: xxx"
```

> 请注意，您只能在 Git 2.20+ 中为别名设置别名。

## shell 别名

Git 别名还可以使用另一个强大的功能：shell 别名。如果在别名值前添加一个 `!`，则可以运行任何 shell 命令。

例如，您可以添加以下别名，以便在每次输入 `git hello` 时打印出 `hello`。

```bash
git config --global alias.hello "!echo hello"
```

## 更好的 git 日志

在我写的另一篇 [Git 日志](https://github.com/lio-zero/blog/blob/main/Git%20%E5%92%8C%20Github/Git%20%E6%97%A5%E5%BF%97.md#%E8%8E%B7%E5%8F%96%E6%BC%82%E4%BA%AE%E7%9A%84%E6%97%A5%E5%BF%97) 中，介绍了如何打印漂亮的 Git 日志。

我们可以将其设置为一个别名：

```bash
git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
```

每次需要查看日志时，可以输入：

```bash
git lg
```

![Git 日志](https://upload-images.jianshu.io/upload_images/18281896-9eba90dfcbd93ae9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

或者，查看更改的行：

```bash
git lg -p
```

![Git 日志](https://upload-images.jianshu.io/upload_images/18281896-3e842e6049c04ec8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 更安全的强制推送

有时，您需要将更改推送到远程存储库并覆盖文件。您可能习惯于使用 `git push --force` 或 `-f`。

但是如果其他人已经将更改推送到同一分支，会发生什么？您的命令将清除他们的提交。**这肯定是不行的**。

Git 有一种更安全的方式来推送更改和覆盖您的提交。您可以使用 `--force with lease`，而不是使用 `--force` 标志。此标志将防止您意外覆盖其他人的提交。

您可以使用 `force-push` 为此编写别名：

```bash
git config --global alias.force-push "push --force-with-lease"
```

现在您可以运行 `git force-push`，这更容易记住。当然，为了节省几次按键操作，您还可以添加此别名的简短版本。

```bash
git config --global alias.fp force-push
```

## 设置默认推送分支名称

默认情况下，推送时使用当前分支的名称作为远程分支的名称。

- 使用 `git config push.default current` 将远程分支的名称设置为当前本地分支的名称作为默认名称。
- 可以使用 `--global` 标志全局配置此选项。

```bash
$ git config --global push.default current

$ git checkout -b my-branch
$ git push -u
# 推送 origin/my-branch
```

## 默认情况下禁用快进合并

- 使用 `git config --add merge.ff false` 禁用所有分支的快进合并。
- 您可以使用 `--global` 标志全局配置此选项。

```bash
$ git config --global --add merge.ff false

$ git checkout master
$ git merge my-branch
# 即使有可能也不会快进合并
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
- 使用 `git config user.name <name>` 为当前储存库设置的用户名。
- 您可以使用 `--global` 标志来配置全局用户信息。

```bash
git config [--global] user.email <email>
git config [--global] user.name <name>
```

配置当前存储库的用户：

```bash
git config user.email "your_email@example.com"
git config user.name "your_name"
```

配置全局 Git 用户：

```bash
git config --global user.email "your_email@example.com"
git config --global user.name "your_name"
```

## 配置存储库的行尾

- 使用 `git config core.eol [lf | crlf]` 配置行尾。
- `lf` 是 UNIX 行的结尾（`\n`），而 `crlf` 是 DOS 行的结尾（`\r\n`）。

```bash
git config core.eol [lf | crlf]
```

配置为使用 UNIX 行结束符：

```bash
git config core.eol lf
```

## 自动更正 Git 命令

为 Git 配置自动更正键入错误的命令。可以使用 `git config --global help.autocorrect 1` 使 Git 的自动更正。

```bash
git config --global help.autocorrect 1
git sttaus # 自动改为 "git status" 运行
```

## 配置 Git 文本编辑器

默认情况下打开 Git 使用的是 `vi` 编辑器，如果你没怎么使用过 UNIX 系统的话，可能不是很熟。你可以通过下面的命令来使用其他编辑器打开 Git。

```bash
git config --global core.editor <editor-command>
```

它通过调用 `<editor-command>` 作为 Git 文本编辑器。

例如，将 VS Code 设置为 Git 文本编辑器：

```bash
git config --global core.editor "code --wait"
```

将 `vi` 作为 Git 的文本编辑器：

```bash
git config --global core.editor "vi"
```

## 编辑 Git 配置文件

使用 `git config -e` 将在默认 Git 文本编辑器（如果未配置）中打开 Git 当前项目的配置文件：

```bash
git config -e
```

如果想查看全局配置文件，可以添加 `--global` 标志：

```bash
git config --global -e
```
