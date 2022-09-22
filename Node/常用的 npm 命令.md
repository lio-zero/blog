# 常用的 npm 命令

使用 npm 时，您很可能会在大多数交互中使用命令行工具。因此，这里详细列出了您将遇到并需要最频繁使用的命令。

## npm 帮助

`npm help` 命令提供了快速查询所有选项：

```bash
npm help
# or
npm --help
```

加上 `--` 是有区别的。第一个将打开默认浏览器，在页面显示信息，而另一个将在命令行输出信息。

它可以帮助我们无论是否在线/离线状态下，都可以快速查看 npm 提供的所有选项。

还可以显示特定 npm 命令的帮助：

```bash
npm help <command>
# or
npm <command> -h
```

## npm 的全局配置文件

npm 配置文件是 `.npmrc`，全局配置文件默认在用户目录下。

如果没有找到，可以通过以下命令查看：

```bash
npm config get userconfig
```

您可以查看该文件的所有 npm 配置信息。

以下是其他一些便捷的命令。

查看所有配置项：

```bash
npm config list
npm config ls
npm config ls  -l
```

查看缓存配置，`get` 后面可以跟任意配置项：

```bash
npm config get cache
```

打开全局 `.npmrc` 文件：

```bash
npm config edit
```

还有一些不错的命令，会在下文讲到。

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

您可以通过 `npm config set` 配置常用的默认字段值：

```bash
npm config set init-author-name "lio-zero"
npm config set init-author-email "licroning@163.com"
npm config set init-author-url "https://github.com/lio-zero"
npm config set init-license "MIT"
# ...
```

> **Tips**：您可以打开全局的 `.npmic` 文件配置统一配置它们。

## 安装其他来源的包

npm cli 还可以让你安装其他来源的包：

```bash
npm config set xxx

# 淘宝镜像
npm config set registry https://registry.npmmirror.com
```

查看是否切换成功：

```bash
npm config get registry
```

如果返回设置的源链接，说明镜像设置成功。

> **Tips**：您可以安装 [nrm](https://github.com/Pana/nrm) 包在不同源之间快速切换，例如 `npm`、`cnpm`、`taobao` 等。

**注意**：原淘宝 npm 域名已停止解析，详细内容查阅[【公告】淘宝 npm 域名即将切换 && npmmirror 重构升级 && 微信交流群](https://zhuanlan.zhihu.com/p/465424728?spm=a2c6h.24755359.0.0.2b394dccWTS9up)。

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

全局安装包：

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
- 它不会安装与 npm install 类似的单个包。

## NPM scripts

`npm scripts` 用于自定义脚本，例如：

```bash
npm run env
```

上面的命令是 `npm cli` 为我们提供的一个脚本命令，用于列出程序内的所有环境变量。

通常，我们可以在 `package.json` 内的 `scripts` 定义我们的脚本命令，例如：

```json
{
  "name": "app-project",
  "scripts": {
    "serve": "nodemon app.js"
  }
}
```

我们可以通过运行 `npm run serve` 来启动我们的应用程序。

我们可以使用不带参数的 `npm run` 命令查看项目上的所有命令脚本：

```bash
npm run

#  serve
#    nodemon app.js
```

> **注意**：每当 `npm run` 命令，都会新建一个 shell 文件来执行我们当前的执行的命令，只要符合 shell 可运行的命令，都可以执行。

在执行 `npm scripts` 的过程中，我们可以通过 `npm_package_xxx` 前缀拿到 `package.json` 内的字段。

例如，获取 `name` 字段：

```js
console.log(process.npm_package_name) // app-project
```

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
npm home <package_name>
# or
npm docs <package_name>

# example
npm home axios
npm home vue
```

导航到 issues：

```bash
npm bug <package_name>
```

打开它的存储库也很容易：

```bash
npm repo <package_name>
```

这几个命令都将在您的默认浏览器中打开目标网站，它们通常会通过 `package.json` 文件所提供的信息进行检索。

## 定位 `node_modules` 目录

```bash
# 本地 node_modules
npm root

# 全局 node_modules
npm root -g
```

## 删除重复包

`npm dedupe` 命令用于删除重复的依赖项。它通过删除重复的包并在多个依赖包之间有效地共享公共依赖项来简化整体结构。它会产生一个扁平的和去重的树。

```bash
npm dedupe
# or
npm ddp
```

## 扫描您的应用程序是否存在漏洞

`npm audit` 检查项目依赖项是否存在漏洞。它可以看出有风险的 package、依赖库的依赖链、风险原因及其解决方案。

```bash
npm audit
```

如果发现存在漏洞，我们可以使用 `npm audit fix`，它将自动安装所有易受攻击依赖包的修补版本（如果可用）。

```bash
npm audit fix
# or
npm audit fix --force
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

`npm doctor` 命令可以在我们的环境中运行多项检查，已检查当前 node 和 npm 存在的问题。

```bash
npm doctor
```

## 在本地测试你的 npm 包

我们可以将本地开发的 npm 包安装到全局或指定目录。

```bash
npm i . -g
# 在某个项目中安装本地包
npm i /path/to/packageName
```

npm 为我们提供了另一个命令， 命令

我们还可以使用 `npm link` 命令做一个软链指向当前需要测试的项目：

```bash
# 先在本地开发的 npm 包中执行
npm link

# 然后切换到你需要安装本地测试包的项目中
npm link <package_name>
```

使用 `npm unlink` 可以取消安装本地的调试包：

```bash
npm unlink <package_name>
```

## 检查过时的包

使用 `npm outdated` 命令来检查所有过时的 npm 包。它还显示了应该为任何过时的包安装的最新版本。

```bash
npm outdated --long
# or
npm outdated -l
```

## 检查任何 npm 包的最新版本

我们可以通过运行以下命令来检查任何已发布的 npm 包的最新版本的详细信息：

```bash
npm view <package_name>
# or
npm v <package_name>
npm info <package_name>
npm show <package_name>
```

仅显示最新版本号：

```bash
npm v <package_name> version
```

显示所有版本的列表：

```bash
npm v <package_name> versions
```

## 列出所有已安装的包

`npm list` 命令可以列出项目中安装的所有 npm 包。它将创建一个树结构，显示已安装的包及其依赖项。

```bash
npm list
#or
npm ls
```

查看单个包：

```bash
npm ls <package_name>
```

可以利用 `--depth flag` 来限制搜索深度

```bash
# 本地
npm ls --depth=1

# 查看全局安装的包
npm list -g --depth 0
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

## 弃用包的相关操作

> **注意**：强烈建议弃用包或包版本而不是取消发布它们，因为取消发布会从注册表中完全删除一个包，这意味着任何依赖它的人都将无法再使用它，而不会发出警告。

弃用整个包：

```bash
npm deprecate <package_name> "弃用信息"
```

弃用包的单个版本：

```bash
npm deprecate <package_name>@<version> "弃用信息"
```

取消弃用操作：

```bash
# 将弃用消息改为空字符串即可
npm deprecate <package_name> ""
```

取消发布整个包：

```bash
npm unpublish <package_name> -f
```

取消发布包的指定版本：

```bash
npm unpublish <package_name>@<version>
```

取消发布包后，以相同名称重新发布将被阻止 24 小时。如果您错误地取消发布了一个包，建议您以不同的名称再次发布，或者对于未发布的版本，增加版本号并再次发布。

## 更新包

`npm update` 命令用于更新包：

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

## 查看哪些包已过时

查看哪些包已过时：

```bash
npm outdated
```

查看全局环境的已过时包：

```bash
npm outdated -g --depth=0
```

## 检测当前镜像源的延迟

检测当前镜像源的延迟情况：

```bash
npm ping
```

## 锁定包依赖

[npm shrinkwrap](https://docs.npmjs.com/cli/shrinkwrap) 命令用于锁定包依赖项的版本，以便您可以准确控制在安装包时将使用每个依赖项的哪些版本。

```bash
npm shrinkwrap
```

它在部署 Node.js 应用程序时非常有用。通过它，您可以确定要部署哪些版本的依赖项。

## 控制项目版本号

当我们发布 npm 包后，每次我们要推送新版本时，都需要修改版本号的，这时可以通过 `npm version` 命令进行设置：

```bash
# 新补丁版本表示向后兼容的错误修复
npm version patch

# 新的次要版本表示向后兼容的新功能
npm version minor

# 新的主要版本表示重大更改
npm version major
```

控制该版本进行升级，注意需要遵循 [Semver 规范](https://semver.org/)。

## 更多资料

- [awesome-npm](https://github.com/sindresorhus/awesome-npm) 收集了很多很棒的 npm 资源和技巧
- [13 npm Tricks for Faster JavaScript Development](https://bretcameron.medium.com/13-npm-tricks-for-faster-javascript-development-4fe2a83f87a2)
- [10 Tips and Tricks That Will Make You an npm Ninja](https://www.sitepoint.com/10-npm-tips-and-tricks/)
- [2018 年了，你还是只会 npm install 吗？](https://juejin.cn/post/6844903582337237006#comment)
