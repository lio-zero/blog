# 常用的 npm 命令

使用 npm 时，您很可能会在大多数交互中使用命令行工具。因此，这里详细列出了您将遇到并需要最频繁使用的命令。

## 快速初始化项目

`npm init` 命令是一个逐步构建项目的工具。

```bash
npm init
```

根据提示填写内容，也可以按提供的默认值一路回车（Enter）。

为了省去上面的操作，我们加上 `--yes` 标志将自动使用默认值 `npm init` 填充所有选项：

```bash
npm init --yes
npm init -y
```

完成以上面操作后，将会生成一个 `package.json` 文件并将其放置在当前目录中。

## 安装其他来源的包

`npm cli` 还可以让你安装其他来源的包：

```bash
$ npm config set xxx

# 淘宝镜像
$ npm config set registry https://registry.npm.taobao.org
```

查看是否切换成功

```bash
npm config get registry
```

如果返回设置的源链接，说明镜像设置成功。

> **建议**：您可以安装 [nrm](https://github.com/Pana/nrm) 包在不同源之间快速切换，例如 `npm`、`cnpm`、`taobao` 等。

## 使用快捷方式安装包

安装依赖项：

```bash
npm install <package_name>
npm i <package_name>
```

安装开发环境依赖：

```bash
npm install --save-dev <package_name>
npm i -D <package_name>
```

安装生产环境依赖（默认）：

```bash
npm install --save-prod <package_name>
npm i -P <package_name>
```

全局安装软件包：

```bash
npm install --global <package_name>
npm i -g <package_name>
```

同时安装多个包：

```bash
npm i express cheerio axios
```

安装具有相同前缀的多个包：

```bash
npm i eslint-{plugin-import,plugin-react,loader} express
```

额外的，当你需要指定包版本时，可以使用 `@` 符号：

```bash
npm i vue@3.2.25
npm i -g webpack@5.58.0
```

检查 npm 包的所有版本，可以在下面的**检查任何 npm 包的最新版本**找到。

## 干净安装你的包依赖

`npm ci` 用于清除安装包依赖项。它通常用于自动化环境，如 CI/CD 平台。

```bash
npm ci
```

它与 `npm install` 有以下不同之处：

- 它安装的是 `package-lock.json` 中提到的包的确切版本。
- 删除现有的 `node_modules` 并运行新的安装。
- 它不会写信给你的 `package.json`或 `*-lock` 文件。
- 它不会安装与 npm install 类似的单个软件包。

## NPM scripts

`npm scripts` 用于自定义脚本，例如：

```bash
npm run env
```

上面的命令是 `npm cli` 为我们提供的一个脚本命令，用于列出程序内的所有环境变量。

通常，我们可以在 `package.json` 内的 `scripts` 定义我们的脚本命令，例如：

```json
{
  "scripts": {
    "serve": "nodemon app.js"
  }
}
```

我们可以通过运行 `npm run serve` 来启动我们的应用程序。

我们可以使用不带参数的 `npm run` 命令查看项目上的所有命令脚本：

```bash
$ npm run

#  serve
#    nodemon app.js
```

> **注意**：每当 `npm run` 命令，都会新建一个 shell 文件来执行我们当前的执行的命令，只要符合 shell 可运行的命令，都可以执行。

在执行 `npm scripts` 的过程中，我们可以通过 `npm_package_` 前缀拿到 package.json 内的字段。

```js
console.log(process.npm_package_name) // app-project
```

`npm script` 还有许多其他的技巧，如通配符、传参、执行顺序、默认值、钩子、简写形式等，关于 `npm scripts` 的详情内容请看阮老师的 [npm scripts 使用指南](http://www.ruanyifeng.com/blog/2016/10/npm_scripts.html)。

## 在 package.json 中配置自己的变量

在上一节中，我们使用到了 `npm run env` 命令，它将列出我们包中存在的所有 npm 环境变量

我们可以在 `package.json` 内的 `config` 字段添加自己的自定义变量：

```json
{
  "config": {
    "myVariable": "Hello World"
  }
}
```

执行以下命令：

```bash
npm run env | grep npm_package_config_
```

我们将看到，我们刚才所配置的变量，它以 `npm_package_config_` 为前缀。

项目中获取自定义变量：

```js
console.log(process.npm_package_config_myVariable) // Hello World
```

> **注意**：`config` 字段内的变量可以在输入命令时被覆盖：

```js
npm config set app-project:myVariable Hello Node
```

## 快速导航到包主页、储存库和 issues

在查找 npm 包的文档时，我们经常使用 Google 搜索其主页和 npm 页面。

但也有其他更快速的方法，我们可以通过运行以下命令快速进入主页：

```bash
$ npm home <package-name>

# example
$ npm home axios
$ npm home vue
```

导航到 issues：

```bash
npm bug <package-name>
```

打开它的存储库也很容易：

```bash
npm repo <package-name>
```

这三个命令都将在您的默认浏览器中打开目标网站。

## npm root 定位全局节点模块目录

```bash
# 本地 node_modules
$ npm root

# 全局 node_modules
$ npm root -g
```

## 删除重复包

`npm dedupe` 命令用于删除重复的依赖项。它通过删除重复的包并在多个依赖包之间有效地共享公共依赖项来简化整体结构。它会产生一个扁平的和去重的树。

```bash
$ npm dedupe
# or
$ npm ddp
```

## 扫描您的应用程序是否存在漏洞

`npm audit` 检查项目依赖项是否存在漏洞。它可以看出有风险的 package、依赖库的依赖链、风险原因及其解决方案。

```bash
npm audit
```

如果发现存在漏洞，我们可以使用 `npm audit fix`，它将自动安装所有易受攻击依赖包的修补版本（如果可用）。

```bash
$ npm audit fix
# or
$ npm audit fix --force
```

更好的做法是使用 [synk](https://github.com/snyk/cli)，它是一个高级版的 `npm audit`，可自动修复，且支持 CI/CD 集成与多种语言。

```bash
npx snyk
```

## 清理 npm 缓存

磁盘满了，试着清楚 npm 缓存：

```bash
npm cache clean --force
yarn cache clean
pnpm store prune
```

## 检查环境

`npm doctor` 命令可以在我们的环境中运行多项检查。

```bash
npm doctor
```

## 在本地测试你的包

```bash
npm link <package_name>
```

## 检查过时的包

使用 `npm outdated` 命令来检查所有过时的 npm 包。它还显示了应该为任何过时的软件包安装的最新版本。

```bash
$ npm outdated --long
# or
$ npm outdated -l
```

## 检查任何 npm 包的最新版本

我们可以通过运行以下命令来检查任何 npm 包的最新版本

```bash
$ npm view <package-name>
# or
$ npm v <package-name>
```

仅显示最新版本

```bash
npm v <package-name> version
```

显示所有版本的列表

```bash
npm v <package-name> versions
```

## 列出所有已安装的包

`npm list` 命令可以列出项目中安装的所有 npm 包。它将创建一个树结构，显示已安装的包及其依赖项。

```bash
$ npm list
#or
$ npm ls
```

- 可以利用 `--depth flag` 来限制搜索深度

```bash
$ npm ls --depth=1

# 查看全局安装的软件包
$ npm list -g --depth 0
```

## 发布一个包

首先，需要使用 `npm login` 登录：

```bash
npm login
```

然后，在使用 `npm publish` 发布项目：

```bash
npm publish
```

> 详细内容可查阅另一篇[如何对 npm package 进行发包](https://github.com/lio-zero/blog/blob/master/Node/%E5%A6%82%E4%BD%95%E5%AF%B9%20npm%20package%20%E8%BF%9B%E8%A1%8C%E5%8F%91%E5%8C%85.md)。

## 更新软件包

`npm update` 命令用于更新软件包：

```bash
npm update <name>
npm update <name> -g
npm update <name> -D
```

为了便于查看依赖信息，我们可以安装 `npm-check` 包，它用于检查过时、不正确和未使用的依赖项。

```bash
npm i -g npm-check
```

运行以下命令：

```bash
npm-check -u
```

它将显示用于选择要更新的模块的交互式 UI。替代的还有 [`npm-check-updates`](https://www.npmjs.com/package/npm-check-updates)。

> 可查看另一篇[检查 npm 模块更新](https://github.com/lio-zero/blog/blob/master/Node/%E6%A3%80%E6%9F%A5%20npm%20%E6%A8%A1%E5%9D%97%E6%9B%B4%E6%96%B0.md)
