# 将本地 Git 标签推送到 GitHub 上的远程仓库

## 将标签推送到 GitHub

在 git 中推送标签类似于推送分支。唯一的区别是，它需要特定的标签名。

```bash
git push -u origin <tag_name>
```

以下示例，将 v0.1 标签推送至 GitHub：

```bash
git push -u origin v0.1

Total 0 (delta 0), reused 0 (delta 0), pack-reused 0
To github.com:lio-zero/xxx.git
 * [new tag]         v0.1 -> v0.1
```

## 检查标签是否已被推送

要检查标签是否仅在本地可用，请运行：

```bash
git push --tags --dry-run
```

`--dry-run` 选项总结了下一次提交中将包含的内容。

如果上述命令的输出状态为 **Everything up-to-date**，则表示没有可推送的标签。

如果上述命令返回如下输出，则表示该标签可以推送。

```bash
To github.com:lio-zero/xxx.git
 * [new tag]         v0.1 -> v0.1
```

## 其他常用命令

- `git tag` 显示本地所有标签
- `git ls-remote --tags` 查看远程所有标签
- `git push -d origin <tag_name>` 删除远程特定标签
- `git tag <tag_name>` 创建轻量标签
- `git tag -a <tag_name> -m "message"` 创建带注释的标签
- `git push --tags` 将所有标签推送到远程仓库
- `git show <tag_name>` 显示特定标签的标签数据
- `git tag -d <tag_name>` 删除特定标签
