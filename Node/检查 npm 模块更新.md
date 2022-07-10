# 检查 npm 模块更新

npm 提供了我们更新模块的命令 `npm update`：

```bash
npm update <name>
npm update <name> -g
npm update <name> -D
```

您可以指定项目所依赖的模块进行更新，但如果我们需要像 yarn 的内置命令 `yarn upgrade-interactive` 一样，查看项目模块详细的更新情况呢？有一些方便模块可以帮助到我们。

## `npm-check-updates` 模块

[`npm-check-updates`](https://www.npmjs.com/package/npm-check-updates) 将你的 `package.json` 依赖升级到最新版本，忽略指定的版本。

```bash
npm i npm-check-updates -g
```

使用 `ncu` 命令检查 npm 模块更新并将为您更新 `package.json`。

![npm-check-updates](https://upload-images.jianshu.io/upload_images/18281896-91bfcf77ef23c111.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

使用 `ncu -u` 命令更新 `package.json` 文件相关依赖，并运行 `npm i` 安装最新包：

```bash
ncu -u
npm i
```

## `npm-check` 模块

为了便于查看依赖信息，我们可以安装 [`npm-check`](https://www.npmjs.com/package/npm-check) 包，它用于检查过时、不正确和未使用的依赖项。

```bash
npm i -g npm-check
```

运行以下命令：

```bash
npm-check -u
```

效果如下：

![npm-check](https://upload-images.jianshu.io/upload_images/18281896-b99ac4b59b74e1f2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

它将显示用于选择要更新的模块的交互式 UI。
