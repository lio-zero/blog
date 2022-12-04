# 什么是 pnpm？

[pnpm](https://pnpm.io/)（Performant NPM，高性能的 npm）是一个新的包管理器，它提出了另一种价值主张，将库代码存储集中到一个中心位置，并将其与你所从事的所有项目共享。

它基本上是 npm 的替代品，这意味着一旦安装了它，就可以调用 `pnpm install` 来下载项目依赖项。

如果你有 10 个使用 Vue 的项目，在同一版本中，pnpm 将安装一次，然后在所有其他项目中引用第一次安装。

这也意味着，与使用标准 npm 过程下载资源相比，项目初始化部分花费的时间要少得多。即使 npm 缓存了包，速度也会更快，因为 pnpm 会硬链接到中央本地存储库，而 npm 会从缓存中复制包。

使用 npm 安装 pnpm：

```bash
npm i -g pnpm
```

然后，作为 pnpm 的替代品，你可以使用所有 npm 命令：

```bash
pnpm i vue
pnpm update vue
pnpm uni vue
# ...
```

pnpm 在那些需要维护大量具有相同依赖项的项目中尤其受欢迎。

除了 npm 常用命令外，pnpm 还提供了一些实用程序，包括 [`pnpm recursive`](https://pnpm.io/cli/recursive)，用于在文件夹中的所有项目中运行相同的命令。例如，你可以通过运行 `pnpm recursive install` 来初始化存储在当前文件夹中的 100 个项目。

如果你使用 npx，这是一种方便（也是推荐）的方式来运行诸如 `create-vue` 之类的实用程序，那么你将通过使用 pnpm 附带的 pnpx 命令获得 pnpm 的好处：

```bash
pnpx create-react-app my-cool-new-app
```

## 软件包安装在哪里？

![pnpm 的优势](https://upload-images.jianshu.io/upload_images/18281896-d70622a859489140.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

上图来自官网，展示了 pnpm 的所创建非扁平化的 `node_modules` 文件夹，也告诉我们软件包安装的具体安装位置。

所有 npm 包都安装在全局目录 `~/.pnpm-store/v3/files` 下（其中 `~` 表示你的主文件夹）。

关于该工具还有许多更高级的知识需要学习，但我希望这有助于你开始使用 pnpm！（你可以在最后提供的资料中学习到它们）

## 你应该转而使用 pnpm 吗？

这里有几点可以供你参考：

- 磁盘空间不足
- 同一仓库管理多个项目（monorepo）

否则，你可以继续使用 npm 或 yarn。

## 最后

遇到问题无法解决，建议看看官网的 [PNPM FAQ](https://pnpm.io/faq) 或 [GitHub Issus](https://github.com/pnpm/pnpm/issues)。

## 更多资料

更加详细的内容可以阅读以下文章：

- [关于现代包管理器的深度思考——为什么现在我更推荐 pnpm 而不是 npm/yarn?](https://juejin.cn/post/6932046455733485575)
- [pnpm 是凭什么对 npm 和 yarn 降维打击的](https://juejin.cn/post/7127295203177676837)
- [包管理工具的演进](https://mp.weixin.qq.com/s/beP1bxgbTT1Z91KS3svDvw)
- [精读《pnpm》](https://github.com/ascoders/weekly/blob/master/%E5%89%8D%E6%B2%BF%E6%8A%80%E6%9C%AF/253.%E7%B2%BE%E8%AF%BB%E3%80%8Apnpm%E3%80%8B.md)
- [pnpm 社区](https://pnpm.io/zh/community/articles) — pnpm 社区整理了众多文章、视频、工具等内容。
