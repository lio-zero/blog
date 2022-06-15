# Git status 的简短版本和不同的 --porcelain 模式

通常，我们需要查看工作区文件的状态，可以使用 `git status`：

```bash
$ git status
Your branch is up to date with 'origin/master'.

Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        deleted:    demo.js
        modified:   package.json

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        config.js

no changes added to commit (use "git add" and/or "git commit -a")
```

但它输出了很多冗长的内容，可能我们只想关心文件及其状态。

我们可以执行带有 `--porcelain` 标志的 `git status` 命令，显示的信息如下：

```bash
$ git status --porcelain
 D demo.js
 M package.json
?? config.js
```

使用带有 `--porcelain` 标志的 `git status` 更有效。但在阅读了 [git status 文档](https://git-scm.com/docs/git-status)之后，我发现还有一个更简便的 `-s` 或 `--short` 标志。

```bash
$ git status -s
# or
$  git status --short
 D demo.js
 M package.json
?? config.js
```

乍一看，它们似乎非常相似，但有一个区别只在终端可见。

![git status --porcelain](https://upload-images.jianshu.io/upload_images/18281896-4fe01be5800c3a0a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![git status -s](https://upload-images.jianshu.io/upload_images/18281896-43a825add2d35225.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

`git status -s` 突出显示文件状态信息的字符。这种高亮显示使阅读更容易。当使用 `--porcelain` 时，这种格式和颜色高亮显示是缺失的。

## `git status -s` 和 `--porcelain` 的区别

**`--porcelain` 标志是用来生成脚本易于解析的输出的。**假设您运行了一些自动化程序，在运行进一步的命令之前检查 `git` 仓库的状态。在这种情况下，您不希望在输出中包含帮助文本和终端颜色。

此外，你也不希望 git 版本之间的输出变化或不同。**`--porcelain` 保证输出不会出现向后不兼容的变化**，这样你的脚本就不会因为 git 更新而中断。

对于包含一些彩色指导的紧凑显示，你可以使用 `git status -s`，如果你要自动化 git 工作流，你可以使用 `git status -porcelain`。

## 不同版本的 `git status --porcelain`

`--porcelain` 有不同的包含版本（**记住它是向后兼容的**)。没有定义的版本 `--porcelain` 默认使用 `v1` 格式。以下是使用 `v2` 输出的内容：

```bash
$ git status --porcelain=v2
1 .D N... 100644 100644 000000 a191d470f17410f90643ef91ba02af6edd6c1fc8 a191d470f17410f90643ef91ba02af6edd6c1fc8 demo.js
1 .M N... 100644 100644 100644 00580fb668d16dda922edf2b05517d15daa7059e 00580fb668d16dda922edf2b05517d15daa7059e package.json
? config.js
```

`v2` 包含更多的附加信息。如果您想了解关于 v2 的更多信息，请访问 [git status 文档](https://git-scm.com/docs/git-status#_porcelain_format_version_2)。
