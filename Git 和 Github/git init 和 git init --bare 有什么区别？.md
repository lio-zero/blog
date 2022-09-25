# git init 和 git init --bare 有什么区别？

- `git init` 创建非裸存储库
- `git init --bare` 创建裸存储库

裸存储库是没有工作副本的 git 存储库，因此 .git 的内容是该目录的顶级内容。

查看以下区别：

![git init](https://upload-images.jianshu.io/upload_images/18281896-acc0ee3a74899419.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![git init --bare](https://upload-images.jianshu.io/upload_images/18281896-c3630be5960c1a8e.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

使用非裸存储库在本地工作，并使用裸存储库作为中央服务器与其他人共享您的更改。例如，当您在 `github.com` 上创建存储库时，它被创建为一个裸存储库。

因此，在您的计算机中：

```bash
git init
touch README
git add README
git commit -m "initial commit"
```

在服务器上：

```bash
cd /www/project
git init --bare
```

在客户端，你推送：

```bash
git push username@server:/www/project main
```

然后，您可以通过将其添加为远程来保存键入内容。

服务器端的存储库将通过拉取和推送的方式获得提交，而不是通过您编辑文件然后在服务器中提交文件，因此它是一个裸存储库。

> 记住：`--bare` 用于远程存储库，而非本地存储库。

## 更多资料

[What is the difference between "git init" and "git init --bare"?](https://stackoverflow.com/questions/7861184/what-is-the-difference-between-git-init-and-git-init-bare)
