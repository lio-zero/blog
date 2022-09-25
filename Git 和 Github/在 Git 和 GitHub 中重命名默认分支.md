# 在 Git 和 GitHub 中重命名默认分支

之前我在 GitHub 创建仓库使用的默认分支为 `master`。最近，我将默认分支切换为 `main`。

为了同步本地分支与远程分支，我们需要采取一些步骤。

下面图示是我在 GitHub 切换仓库的默认分支时，提供给我们如何同步本地分支与远程分支的步骤。

![重命名默认分支](https://upload-images.jianshu.io/upload_images/18281896-0dc6ae649d923d99.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

以下是上图 👆 命令的大致分析。

使用 `-m` 标志在本地移动默认分支。

```bash
git branch -m master main
```

上面的 `master` 是旧分支名称，`main` 是新分支名称。

拉取最新代码，并建立本地分支与远程分支的追踪关系：

```bash
git fetch origin
git branch -u origin/main main
```

将当前本地 HEAD 分支设置为指向 GitHub 上的新分支。

```bash
git remote set-head origin main
# or
git remote set-head origin -a
```

你可以显示的设置新的分支名，也可以使用 `-a` 将自动查询远程以确定其 HEAD。

现在，您可以正常的推送消息了。

但是如果你不知道远程默认分支名已被修改，您未切换和跟踪默认分支。这时您推送了新的提交，本来仓库的 `master` 分支就没了，这时推送会再次新建 `master` 分支。可以运行以下命令以删除旧的默认分支名。

```bash
$ git push origin --delete master

To github.com:lio-zero/mail-push.git
 - [deleted]         master
```
