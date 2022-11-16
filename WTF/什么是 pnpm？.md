# 什么是 pnpm？

[pnpm](https://pnpm.io/) 是一个新的包管理器，它提出了另一种价值主张，将库代码存储集中到一个中心位置，并将其与你所从事的所有项目共享。

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

在 window 中，在 `~/.pnpm-store/` 文件夹中（其中 `~` 表示你的主文件夹）。

我以安装 `lodash` 为例，得到的文件夹结构如下：

```bash
$ tree .pnpm-store/
.pnpm-store/
└── 2
  ├── _locks
  ├── registry.npmjs.org
  │ └── lodash
  │ ├── 4.17.11
  │ │ ├── integrity.json
  │ │ ├── node_modules
  │ │ │ └── lodash
  │ │ │   ├── ...
  │ │ ├── package -> node_modules/lodash
  │ │ └── packed.tgz
  │ └── index.json
  └── store.json
```

关于该工具还有许多更高级的知识需要学习，但我希望这有助于你开始使用 pnpm！

## 你应该转而使用 pnpm 吗？

这里有几点可以供你参考：

- 磁盘空间不足
- 同一仓库管理多个项目（monorepo）

否则，你可以继续使用 npm 或 yarn。

## 更多资料

更加详细的内容可以阅读以下文章：

- [关于现代包管理器的深度思考——为什么现在我更推荐 pnpm 而不是 npm/yarn?](https://juejin.cn/post/6932046455733485575)
- [pnpm 是凭什么对 npm 和 yarn 降维打击的](https://juejin.cn/post/7127295203177676837)
