# Git hooks

Git hooks 是每次 Git 存储库中发生特定事件时自动运行的脚本。它们允许您定制 Git 的内部行为，并在开发生命周期的关键点触发可定制的操作。

![在提交创建过程中执行的 hooks](https://wac-cdn.atlassian.com/dam/jcr:ac22adee-d740-4216-a92a-33c14b5623e5/01.svg?cdnVersion=396)

Git hooks 的常见用例包括鼓励提交策略、根据存储库的状态改变项目环境，以及实现连续集成工作流。但是，由于脚本是无限可定制的，您可以使用 Git hooks 来自动化或优化开发工作流的几乎任何方面。但如果添加的 Hook 脚本未正常运行，则 Git 操作将不会通过。

Hook 脚本置于目录 `~/.git/hooks` 中，以可执行文件的形式存在。

```bash
$ ls -lah .git/hooks
applypatch-msg.sample     pre-push.sample
commit-msg.sample         pre-rebase.sample
fsmonitor-watchman.sample pre-receive.sample
post-update.sample        prepare-commit-msg.sample
pre-applypatch.sample     update.sample
pre-commit.sample         push-to-checkout.sample
pre-merge-commit.sample
```

以上常用 hooks 有：

- `pre-commit`
- `pre-push`
- `commit-msg`

另外 git hooks 可使用 `core.hooksPath` 自定义脚本位置。

```bash
# 可通过命令行配置 core.hooksPath
$ git config 'core.hooksPath' .husky

# 也可通过写入文件配置 core.hooksPath
$ cat .git/config
[core]
  hooksPath = .husky
```

在前端工程化中，husky 即通过自定义 [`core.hooksPath`](https://git-scm.com/docs/git-config#Documentation/git-config.txt-corehooksPath) 并将 `npm scripts` 写入其中的方式来实现此功能。

[Husky](https://github.com/typicode/husky) 允许您安装钩子，包括用 JavaScript 编写的钩子。

`~/.husky` 目录下手动创建 hook 脚本。

```bash
# 手动创建 pre-commit hook
$ vim .husky/pre-commit
```

在 `pre-commit` 中进行代码风格校验：

```bash
#!/bin/sh

npm run lint
```

## 更多资料

- [Git Hooks](https://www.atlassian.com/git/tutorials/git-hooks)
- [Can Git hook scripts be managed along with the repository?](https://stackoverflow.com/questions/427207/can-git-hook-scripts-be-managed-along-with-the-repository)
- [How I Learned to Stop Worrying and Love Git Hooks](https://css-tricks.com/how-i-learned-to-stop-worrying-and-love-git-hooks/)
