# 设置 Git SSH 密钥

使用命令行使用 Git 时，最常见的处理身份验证的方法是使用 SSH 密钥。

大多数基于 GUI 的客户端（如 [GitHub Desktop](https://desktop.github.com/)）都可以为您处理这个问题，但有时您需要命令行，因此设置 SSH 密钥非常有用。

此外，有时您需要 SSH 密钥来执行有用的操作，例如在远程服务器上拉取存储库。

## 计算机上的密钥

SSH 密钥存储在 `~/.ssh` 文件夹中。

您可以在其中拥有多个密钥，因为 SSH 密钥用于 Git 以外的其他事物。

您可以通过以下命令列出所有 SSH 密钥：

```bash
ls -al ~/.ssh
```

如果你有现有的密钥，你会注意到它们成对存在，一个文件和另一个类似命名的以 `.pub` 结尾的文件：

`.pub` 文件包含公钥，而另一个文件包含不应在任何地方共享的私钥。

您不应该在任何地方共享私钥。如果丢失了私钥，则必须重新生成新的私钥/公钥对，因为没有私钥部分，身份验证无法成功完成。

## 生成新密钥

您可以使用 `ssh-keygen` 命令生成一个新的 SSH 密钥。它适用于所有 macOS、Linux 和现代 Windows 计算机，带有 Linux 子系统或 Git for Windows 包。

以下是您使用的命令：

```bash
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

最后一部分（在本例中填充了电子邮件地址）是注释。您可以输入任何您想要的电子邮件，它不必是您的 GitHub 帐户，甚至可以是一个随机字符串。如果可能存在歧义，了解谁生成了密钥会很有用。

密钥生成程序将询问您要将密钥保存在哪里。如果这是第一个密钥，建议您使用 `id_rsa` 作为文件名，但您最好选择一个能够记住为其生成服务的文件名，例如 `github_rsa`。

您可以选择添加密码。我强烈建议设置密码。macOS 会将密码存储在钥匙链中，这样您就不必每次都重复。

## 将密钥添加到 GitHub

在 GitHub **Setting** 中，找到 **SSH and GPG keys** 菜单，并点击 **New SSH key**：

![New SSH key](https://upload-images.jianshu.io/upload_images/18281896-94295e0b44fe9f08.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

设置一个有意义的标题，并在 **key** 部分填上，上面生成的秘钥。

您可以打开 `.pub` 密钥文件，复制其内容并将其粘贴到此框中。也可以使用任何 CLI 命令（如 `cat id_rsa.pub`）来执行此操作。

一旦您将该字符串保存到 GitHub，您的 Git 客户端将拥有与删除服务器通信所需的凭据。

## 使用多个键

建议对每个要使用的服务使用不同的 SSH 密钥。

如果您决定更新它，则可以很容易地使特定服务上的密钥无效，而无需在您使用的所有服务上更改它，无论是因为泄露/公开暴露其他其他原因。

## 更多资料

[为不同的 GitHub 帐户使用多个 SSH 密钥](https://github.com/lio-zero/blog/blob/main/Git%20%E5%92%8C%20Github/%E4%B8%BA%E4%B8%8D%E5%90%8C%E7%9A%84%20GitHub%20%E5%B8%90%E6%88%B7%E4%BD%BF%E7%94%A8%E5%A4%9A%E4%B8%AA%20SSH%20%E5%AF%86%E9%92%A5.md)
