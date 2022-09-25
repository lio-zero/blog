# Git .gitignore

[`.gitignore`](https://git-scm.com/docs/gitignore) 用于指定有意忽略的未跟踪文件。

## Git 忽略文件或目录

与他人共享您的代码时，通常有您不想共享的文件或项目的一部分。例如：

- 日志文件
- 临时文件
- 隐藏文件
- 个人档案
- etc

Git 可以使用 `.gitignore` 文件指定 Git 应忽略项目的哪些文件或目录。

Git 不会跟踪 `.gitignore` 中指定的文件和文件夹。但是，`.gitignore` 文件本身由 Git 跟踪。

## 创建 `.gitignore`

转到项目的根目录，然后创建 `.gitignore` 文件：

```bash
touch .gitignore
```

在文件中添加两条规则：

- 忽略任何带有 `.log` 扩展名的文件
- 忽略任何名为 `temp` 的目录中的所有内容

```txt
*.log
temp/
```

现在，Git 将忽略所有 `.log` 文件和 `temp` 文件夹中的任何内容。

另外，在这种情况下，我们使用单个 `.gitignore` 适用于整个存储库。但也可以有额外的子目录中的 `.gitignore` 文件。这些仅适用于该目录中的文件或文件夹。

## .gitignore 的规则

`.gitignore` 文件中匹配模式的基本规则可以查阅 [PATTERN FORMAT](https://git-scm.com/docs/gitignore#_pattern_format)。

## 本地和个人 Git 忽略规则

也可以忽略文件或文件夹，但不在分布式 `.gitignore` 文件中显示。

这些类型的忽略在 `.git/info/exclude` 文件中指定。它的工作原理与 `.gitignore` 相同，但不会显示给其他任何人。

## 全局排除文件

我们可以在全局创建一个 `.gitignore_global` 文件，用于每个储存库排除一些文件。

运行以下命令以定义 `.gitignore_global` 为 `core.excludesfile` 选项：

```bash
git config --global core.excludesfile ~/.gitignore_global
```

此命令在全局 `.gitconfig` 中创建以下设置：

```bash
# .gitconfig
[core]
  excludesfile = ~/.gitignore_global
```

创建一个 `.gitignore_global` 文件，并添加要排除的文件。

```bash
# .gitignore_global
.DS_Store
```

您永远不必担心 `DS_Store` 文件再次提交！
