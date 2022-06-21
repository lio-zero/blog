# Git hooks

Git 允许在各种操作之前添加一些 Hook 脚本，如未正常运行，则 Git 操作不通过。

而 Hook 脚本置于目录 `~/.git/hooks` 中，以可执行文件的形式存在。

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

以上常用 hook 有：

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

```sh
#!/bin/sh

npm run lint
```

## 更多资料

[Can Git hook scripts be managed along with the repository?](https://stackoverflow.com/questions/427207/can-git-hook-scripts-be-managed-along-with-the-repository)
