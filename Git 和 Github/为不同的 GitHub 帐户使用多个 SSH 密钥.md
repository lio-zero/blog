# 为不同的 GitHub 帐户使用多个 SSH 密钥

您通常使用 SSH 密钥来访问 GitHub 存储库，而不是输入用户名和密码，也就是免密码登录。这是与远程 GitHub 服务器通信的一种更安全、更推荐的方式。

有时候你有不止一个 GitHub 账户。例如，一个用于访问个人存储库，另一个用于您的日常工作。

问题是本地 Git 如何重新授权 GitHub 帐户附带的存储库。本文可能对您有所帮助。

## 创建不同的密钥

假设 `foo` 和 `bar` 是您希望在同一台计算机中使用的两个 GitHub 用户名。您可以按照[官方的 GitHub 指南生成 SSH 密钥](https://docs.github.com/en/github/authenticating-to-github/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)。

```bash
# 为 foo 生成 SSH 密钥
$ ssh-keygen -t ed25519 -C "foo@domain.com"
```

当要求您指定要保存密钥的文件时，不要使用默认密钥。将文件名更改为与帐户关联的名称，例如：

```bash
Enter a file in which to save the key (/home/you/.ssh/id_ed25519):
`/home/you/.ssh/id_foo`
```

对 `bar` 帐户重复相同的步骤。现在，我们有两个私钥，`id_foo` 和 `id_bar` 位于 `~/.ssh` 文件夹中。

## 向 SSH 代理添加密钥

```bash
# 删除缓存的密钥
$ ssh-add -D

# 在后台启动 ssh-agent
$ eval "$(ssh-agent -s)"

# 将 SSH 私钥添加到 ssh-agent
$ ssh-add ~/.ssh/id_foo
$ ssh-add ~/.ssh/id_bar
```

## 将密钥映射到 GitHub repo

这一步让 SSH 知道应该为特定主机使用哪个私钥。

```bash
cd ~/.ssh
touch config
```

将以下内容添加到 `config` 文件：

```bash
Host github.com-foo
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_foo
  IdentitiesOnly yes

Host github.com-bar
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_bar
  IdentitiesOnly yes
```

您会发现，`github.com-foo` 和 `github.com-bar` 看起来是无效的主机，但实际上它们被视为别名。SSH 使用 `HostName` 选项对其进行映射，并在 `IdentityFile` 选项中使用私钥。

## 更改 GitHub 设置

假设 `foo` 帐户访问一个 GitHub 存储库，其 URL 为 `github.com/foo/a-foo-repos`。转到其克隆文件夹，并更改 `.git/config` 文件，如下所示。

```bash
[remote "origin"]
  url = git@github.com-foo:foo/a-foo-repos.git
```

注意，这里使用了在上一步中创建的 SSH Host `github.com-foo`：

为 `bar` 存储库应用类似的设置。
