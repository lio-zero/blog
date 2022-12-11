# package.json 详解

当我们创建一个 Node 项目时，需要创建一个 `package.json` 文件，它描述项目所需要的基本信息（如项目名、作者、版本、许可证等元数据）、脚本命令、依赖等。

你可以在命令行使用 `npm help package.json` 命令，将使用默认浏览器打开页面，显示有关 `package.json` 文件中所需内容的所有信息。

> **推荐**：以下的所以 npm 命令，你可以在[常用的 npm 命令](https://github.com/lio-zero/blog/blob/main/Node/%E5%B8%B8%E7%94%A8%E7%9A%84%20npm%20%E5%91%BD%E4%BB%A4.md)这篇文章中了解到。

## 基本字段

开始一个新项目时，我们会在命令行使用 `npm init` 命令生成 `package.json` 文件。

以下是 `package.json` 的一些字段信息：

`name` 项目的名称。

```json
{
  "name": "node"
}
```

`version` 项目的版本号。

```json
{
  "version": "0.1.0"
}
```

`description` 项目的描述信息。

```json
{
  "description": "My package"
}
```

`main` 字段指定项目的主入口文件。如果没有设置 `main`，它默认为项目根文件夹中的 `index.js`。

```json
{
  "main": "index.js"
}
```

当我们导入某个包时，实际上导入的是 `main` 字段所指向的文件。

```js
// 将根据 node_modules/ws 下 package.json 文件的 main 字段，找到入口文件
const ws = require('ws')
```

> **Tips**：`main` 是 CommonJS 时代的产物，也是最古老且最常用的入口文件。

`author` 字段指定项目的作者信息。它可以有两种写法：

```json
// 方式一：
{
  "author": {
    "name" : "lio-zero",
    "email" : "licroning@163.com",
    "url" : "https://github.com/lio-zero/blog"
  }
}

// 方式二：
{
  "author": "lio-zero <licroning@163.com> (https://github.com/lio-zero/blog)"
}
```

`keywords` 使用相关的关键字描述项目。

```json
{
  "keywords": ["package.json", "node"]
}
```

`license` 字段指定项目许可证，告诉用户如果使用当前项目，以及使用的一些限制。常见的许可证有 MIT、BSD-3-Clause 等。

```json
{
  "keywords": "MIT"
}
```

`scripts` 字段指定项目的脚本命令。例如以下 `start` 指定了运行 `npm run start` 时，所要执行的命令。

```json
{
  "scripts": {
    "start": "node ./bin/xxx"
  }
}
```

> `scripts` 有一些很重要的声明周期钩子，例如 `prepublishOnly`。具体请查阅：[NPM scripts](https://github.com/lio-zero/blog/blob/main/Node/%E5%B8%B8%E7%94%A8%E7%9A%84%20npm%20%E5%91%BD%E4%BB%A4.md#npm-scripts)。

`repository` 字段用于指定项目的代码仓库地址。

```json
{
  "repository": {
    "type": "git",
    "url": "https://github.com/lio-zero/blog"
  }
}
```

以上是使用 `npm init` 时，提示你需要录入的信息。

你也可以添加 `-y` 标志来生成默认的 `package.json` 文件：

```json
{
  "name": "demo",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC"
}
```

使用 `npm init -y` 默认生成的 `package.json` 文件会少一个 `repository` 字段，需要的话手动添加上去即可。

接下来，我们将在这基础上介绍一些其他字段，它们都是可选的，也会在你使用某些 npm 命令时自动生成。

## 项目依赖字段

[`dependencies`](https://github.com/npm/npm/blob/2e3776bf5676bc24fec6239a3420f377fe98acde/doc/files/package.json.md#dependencies) 字段指定了生产环境项目的依赖。当你添加生产环境依赖时，它会自动生成，如：`npm i express`。

```json
{
  "dependencies": {
    "express": "^4.17.1"
  }
}
```

[`devDependencies`](https://github.com/npm/npm/blob/2e3776bf5676bc24fec6239a3420f377fe98acde/doc/files/package.json.md#devdependencies) 字段记录了项目在开发环境的依赖关系。当你添加开发环境依赖时，该字段会自动生成并添加相关依赖。例如执行 `npm i eslint -D` 命令，将自动生成以下内容：

```json
{
  "devDependencies": {
    "eslint": "^6.7.2"
  }
}
```

[`peerDependencies`](https://github.com/npm/npm/blob/2e3776bf5676bc24fec6239a3420f377fe98acde/doc/files/package.json.md#peerdependencies) 字段用于指定项目所依赖的其他项目的版本要求。如果你安装的是一个插件，非常适合这种方式。

```json
{
  "peerDependencies": {
    "tea": "2.x"
  }
}
```

[`optionalDependencies`](https://github.com/npm/npm/blob/2e3776bf5676bc24fec6239a3420f377fe98acde/doc/files/package.json.md#optionaldependencies) 如果你想在某些依赖即使没有找到，或者安装失败的情况下，npm 都可以继续执行，那么这些依赖适合放在这里。

```json
{
  "optionalDependencies": {}
}
```

`bundledDependencies` 字段用于指定项目打包时需要包含的依赖库。这意味着，当用户安装你的包时，这些依赖也会被安装，并与你的包一起发布。

例如，如果你的包依赖于 `lodash`，并且你希望它们一起发布，你可以在 `package.json` 文件中添加以下代码：

```json
{
  "bundledDependencies": ["lodash"]
}
```

这样，当用户安装你的包时，`lodash` 也会被安装。如果你的包使用这些依赖，这样做会更方便，因为用户只需安装一个包即可获得所有依赖。

文档中有详细介绍它们，Stack Overflow 上也有关于这几个字段的详细介绍，请查阅 [What's the difference between dependencies, devDependencies and peerDependencies in npm package.json file?](https://stackoverflow.com/questions/18875674/whats-the-difference-between-dependencies-devdependencies-and-peerdependencies/22004559#22004559) 和 [Advantages of bundledDependencies over normal dependencies in npm](https://stackoverflow.com/questions/11207638/advantages-of-bundleddependencies-over-normal-dependencies-in-npm)。

## `module` 字段

`module` 字段表示作为 ES 模块的入口。

```json
{
  "module": "./index.mjs"
}
```

前端工程化下，很多项目会选择一次打包成多种文件格式，如 `cjs/mjs`。当你使用 `import` 导入模块时，它将找到 `module` 指向 ESM 的入口文件，如果使用 `require` 导入模块，将找到 `main` 指向 CJS 的入口文件。

```js
{
  "name": 'app-project',
  "main": './index.cjs',
  "module": './index.mjs'
}
```

如果你只发布一份 ESM 模块化方案，则直接使用 `main` 字段即可。

## `exports` 字段

<!-- `exports` 字段可以控制子目录的访问路径。 -->

`exports` 字段允许你将一个包中的脚本文件、数据文件和其他资源导出为一个模块，供其他人使用。

例如，如果你有一个 JavaScript 包，它包含一个名为 `lib.js` 的脚本文件和一个名为 `data.json` 的数据文件，你可以使用以下代码将它们导出为模块：

```json
{
  "exports": {
    "./lib.js": "./lib.js",
    "./data.json": "./data.json"
  }
}
```

这样，其他人就可以通过 `require()` 方法引用这些文件：

```js
const lib = require('./lib.js')
const data = require('./data.json')
```

但请注意，这种方式只能用于导出包内的文件。

对于导出整个包或自定义导出，应使用其他方法，例如 CommonJS 模块系统或 ECMAScript 模块。

`exprots` 字段还有一个作用，它可以将 CommonJS 模块或 ECMAScript 模块放在同一个包中，并指定多个条目以有条件地提供这些格式，Node 将根据用户或下游包环境解析使用哪种模块系统。

```js
{
  "name": "package-name",
  "main": './index.cjs',
  "module": './index.mjs',
  "exports": {
    ".": {
      "require": "./index.cjs", // CJS
      "import": "./index.mjs"   // ESM
    }
  }
}
```

它将替换 `main` 或 `module` 字段所指向的入口文件，且只有在 `exports` 中声明的文件可以被使用。

[Types for sub modules](https://antfu.me/posts/types-for-sub-modules)

> 关于 `exports` 字段，可以查阅 webpack 的 [Package exports](https://webpack.docschina.org/guides/package-exports/) 章节。

## `type` 字段

`type` 字段指定了包的类型，可以是 `commonjs`、`module` 或 `unpkg`。

- `commonjs` — 表示包使用 CommonJS 模块系统，这是最常见的模块类型。
- `module` — 表示包使用 ECMAScript 模块系统。
- `unpkg` — 表示包可以通过 [unpkg.com](https://www.unpkg.com/) 网站加载。

例如，如果你的包使用 ECMAScript 模块系统，你可以在 `package.json` 文件中添加以下代码：

```js
{
  "type": "module"
}
```

这样，当其他人安装你的包时，npm 就会知道它应该如何处理模块。

但请注意，`type` 字段是可选的，如果不指定，npm 会自动检测包的模块类型。但是，建议明确指定包的模块类型，以避免混淆。

## `funding` 字段

`funding` 字段用于指定包作者或维护者的资金来源信息。这些信息会显示在 npm 包页面上，供用户参考。

例如，如果你的包由 [OpenCollective](https://opencollective.com/) 提供资金支持，你可以在 `package.json` 文件中添加以下代码：

```json
{
  "funding": {
    "type": "OpenCollective",
    "url": "https://OpenCollective.com/my-package"
  }
}
```

这样，当用户查看你的包时，他们就可以点击链接，访问 OpenCollective 网站，了解更多有关资金支持的信息。

但请注意，`funding` 字段是可选的，如果不指定，npm 不会显示任何资金来源信息。但是，建议提供这些信息，以便用户可以更好地了解你的包和资金支持情况。

## `workspaces` 字段

`workspaces` 字段用于管理多个包之间的依赖关系。它可以让你在一个父包中组织多个子包，并定义它们之间的依赖关系。

例如，如果你有一个父包 `my-workspace`，包含两个子包 `package-a` 和 `package-b`，你可以在 `package.json` 文件中添加以下代码：

```json
{
  "workspaces": ["package-a", "package-b"]
}
```

这样，当你在父包中安装依赖时，npm 会自动安装子包的依赖，并将它们组织在一起。这样，你就可以通过父包来管理整个工作区间，而不必在每个子包中都定义依赖。

但请注意，`workspaces` 字段是可选的，如果不指定，npm 不会管理工作区间。但是，如果你有多个包需要组织在一起，建议使用 `workspaces` 字段来管理它们。

## `config` 字段

`config` 字段用于存储配置信息。它允许你在 `package.json` 文件中定义默认配置，并在运行时使用这些配置。

例如，如果你的包需要一个名为 `port` 的配置，并且默认值为 5000，你可以在 `package.json` 文件中添加以下代码：

```json
{
  "name": "node",
  "config": {
    "port": 5000
  }
}
```

这样，你的包就可以在运行时读取这个配置：

```js
const port = process.env.npm_package_config_port
```

你还可以使用下面命令改变这个值。

```bash
npm config set node:port 8080
```

## `bin` 字段

`bin` 字段用于指定包的可执行文件。它允许你将包安装为命令行工具，并在命令行中执行。

例如，如果你的包包含一个名为 `magic` 的可执行文件，你可以在 `package.json` 文件中添加以下代码：

```json
{
  "bin": {
    "magic": "./bin/magic"
  }
}
```

这样，当用户安装你的包时，`magic` 命令就可以在命令行中使用了。用户可以通过运行 `magic` 命令来执行可执行文件。

如果你的包可以作为命令行工具使用，建议使用 `bin` 字段来指定可执行文件。

请注意，这些可执行文件的头部需要加上 `#!/usr/bin/env node`。

## `publishConfig` 字段

`publishConfig` 字段用于存储发布配置信息。它允许你在 `package.json` 文件中定义特定于发布的配置，例如发布源或发布标签。

例如，如果你希望发布你的包到 `my-registry` 源，并使用 `latest` 标签，你可以在 `package.json` 文件中添加以下代码：

```json
{
  "publishConfig": {
    "registry": "https://my-registry/",
    "tag": "latest"
  }
}
```

这样，当你运行 `npm publish` 命令时，npm 会使用这些配置来发布你的包。

请注意，`publishConfig` 字段是可选的，如果不指定，npm 会使用默认的发布配置。但是，如果你希望定义特定于发布的配置，建议使用 publishConfig 字段。

## `files` 字段

`files` 字段用于指定哪些文件应该被发布。它允许你控制哪些文件会被包含在发布版本中，以及哪些文件会被忽略。如果需要把某些文件不包含在项目中，可以添加一个 `.npmignore` 文件。这个文件和 `gitignore` 类似。

例如，你希望仅发布包的 `dist` 文件夹，则可以在 `package.json` 文件中添加以下代码：

```json
{
  "files": ["dist"]
}
```

这样，当你运行 `npm publish` 命令时，整个包仅有 `dist` 文件夹被包含在发布版本中。

你还可以使用 `!` 表示不包含哪些文件/文件夹到发布版本中：

```json
{
  "files": ["!src"]
}
```

请注意，`files` 字段是可选的，如果不指定，npm 会将包根目录下的所有文件都发布。但是，如果你希望控制发布的文件，建议使用 `files` 字段。

## 其他

`homepage` 项目首页的网址。

```json
{
  "homepage": "https://github.com/lio-zero/blog"
}
```

`bugs` 字段用于指定项目的 Issues 追踪系统的 URL 或邮箱地址（bug 报告地址），这些对遇到包存在问题的人很有帮助。

```json
{
  "bugs": {
    "url": "http://github.com/owner/project/issues",
    "email": "project@hostname.com"
  }
}
```

`private` 字段用于指定项目是否为私有项目。如果设置为 `true`，则表示该项目不能被公开发布。该字段可以确保包不会被发布到 npm 上。这可以防止私有项目不小心被发布出去。

```json
{
  "private": true
}
```

`preferGlobal` 字段用于指示 npm 是否应该将包安装为全局包。如果设置为 `true`，当用户安装包时，npm 会提示用户安装为全局包。

```json
{
  "preferGlobal": true
}
```

`browserslist` 用于指定项目所支持的浏览器版本。这个字段通常与浏览器兼容性相关的工具（例如 Babel 或 Autoprefixer）配合使用，以便确保项目在指定的浏览器版本中运行正常。

```json
{
  "browserslist": [
    "last 3 Chrome versions",
    "last 3 Firefox versions",
    "Safari >= 10",
    "Explorer >= 11"
  ]
}
```

`engines` 字段用于指定运行当前项目所需要的 Node.js 版本，也可以指定适用的 `npm` 版本。

```json
{
  "engines": {
    "node": ">= 14.0.0",
    "npm": ">= 6.14.8"
  }
}
```

当本地的 Node 版本与项目的版本不一致，npm 在安装依赖时，将会发出警告，提示你本地的 Node 版本与此项目不符，而 yarn 将直接报错。还有一点很重要，当某些依赖所使用的版本号与项目运行的 Node 版本不一致时，yarn 会也报错，此时项目无法正常运行，可避免意外发生。显示 yarn 这方面做的很好。

`man` 字段用于指定包的手册页。它允许你指定可以通过 `man` 命令访问的手册页文件，以便用户可以查看你的包的相关文档。

```json
{
  "man": ["./doc/calc.1"]
}
```

`style` 字段用于指定供浏览器使用时，样式文件所在的位置。样式文件打包工具 [parcelify](https://github.com/rotundasoftware/parcelify)，通过它知道样式文件的打包位置。

```json
{
  "style": ["./node_modules/tipso/src/tipso.css"]
}
```

`cpu` 字段用于指定项目的架构（例如：x64 或 ia32）

```json
{
  "cpu": ["x64", "ia32"],
  "cpu": ["!arm", "!mips"]
}
```

`os` 字段用于指定项目字段可以用来指定，可以在什么操作系统上运行。

```json
{
  "os": ["darwin", "linux"]
}
```

`engineStrick` 是一个布尔值。如果你肯定你的程序只能在制定的 engine 上运行，设置为 true。

还有一些特殊的字段，比如 `lint-staged`、`husky`、`eslint`、`prettier`、`unpkg`、`babel`、`jest` 等，它们需要配合相应的依赖一起使用。上面的 `browserslist` 字段就是其中之一。

## 根据 `package.json` 内容自动生成 `README.md` 文件

对于任何开源项目来说，最重要的文档就是 README。

这里介绍一个好用的工具 [`readme-md-generator`](https://github.com/kefranabg/readme-md-generator)，它将根据 `package.json` 文件内容自动生成漂亮 README 文件。

`readme-md-generator` 能够读取你的环境（`package.json`、git 配置等），自动创建项目的 `README.md` 文件。

以下是该库提供的一个示例演示：

![readme-md-generator 演示](https://upload-images.jianshu.io/upload_images/18281896-f3c63011175001bf.gif?imageMogr2/auto-orient/strip)

### 基本配置

确保已安装 [npx](https://www.npmjs.com/package/npx)（npx 从 npm `5.2.0` 开始默认发布）

只需在项目根部运行以下命令并回答问题：

```bash
npx readme-md-generator
```

或使用 `-y` 标志默认生成：

```bash
npx readme-md-generator -y
```

假设我们有如下配置：

```json
{
  "name": "vue3",
  "version": "0.1.0",
  "private": true,
  "description": "vue3-demo",
  "author": "lio-zero <licroning@163.com>",
  "license": "MIT",
  "engines": {
    "node": ">= 12.16.2",
    "npm": ">= 6.14.8"
  },
  "repository": {
    "type": "git",
    "url": "git+https://github.com/lio-zero/vue3.git"
  },
  "homepage": "https://github.com/lio-zero/vue3.git#readme",
  "keywords": ["vue3", "cli"],
  "scripts": {},
  "dependencies": {},
  "devDependencies": {}
}
```

效果如下：

![生成 README.md](https://upload-images.jianshu.io/upload_images/18281896-4b95e028b19bdedb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

> 详细内容请查阅文档，例如可以单独定制模板等。
>
> 更新：还有一个不错的工具 [readme.so](https://readme.so/)，允许你通过简单的编辑器快速添加和自定义项目 README 文件所需的所有部分。

## 更多资料

- [npm Docs - package-json](https://docs.npmjs.com/cli/v8/configuring-npm/package-json)
- [package.json](http://caibaojian.com/npm/files/package.json.html)
- [What's what? - Package.json cheatsheet!](https://areknawo.com/whats-what-package-json-cheatsheet/)
