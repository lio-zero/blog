# package.json 详解

当我们创建一个 Node 项目时，需要创建一个 `package.json` 文件，它描述项目所需要的基本信息（如项目名、作者、版本、许可证等元数据）、脚本命令、依赖等。

你可以在命令行使用 `npm help package.json` 命令，将使用默认浏览器打开页面，显示有关 `package.json` 文件中所需内容的所有信息。

> **推荐**：以下的所以 npm 命令，你可以在[常用的 npm 命令](https://github.com/lio-zero/blog/blob/main/Node/%E5%B8%B8%E7%94%A8%E7%9A%84%20npm%20%E5%91%BD%E4%BB%A4.md)这篇文章中了解到。

## 字段信息

开始一个新项目时，我们会在命令行使用 `npm init` 命令生成 `package.json` 文件。

以下是 `package.json` 的一些字段信息：

`name` 项目名。

```json
{
  "name": "node"
}
```

`version` 描述项目的当前版本号。

```json
{
  "version": "0.1.0"
}
```

`description` 项目的描述。

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

`scripts` 指定运行脚本命令的 npm 命令行缩写，比如 `start` 指定了运行 `npm run start` 时，所要执行的命令。

```json
{
  "scripts": {
    "start": "node ./bin/xxx"
  }
}
```

> `scripts` 有一些很重要的声明周期钩子，例如 `prepublishOnly`。具体请查阅：[NPM scripts](https://github.com/lio-zero/blog/blob/main/Node/%E5%B8%B8%E7%94%A8%E7%9A%84%20npm%20%E5%91%BD%E4%BB%A4.md#npm-scripts)。

`repository` 字段用于指定代码存放的位置。

```json
{
  "repository": {
    "type": "git",
    "url": "https://github.com/lio-zero/blog"
  }
}
```

以上是使用 `npm init` 时，提示你需要录入的信息。

您也可以添加 `-y` 标志来生成默认的 `package.json` 文件：

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

接下来，我们将在这基础上介绍一些其他字段，他们都是可选的，也会在您使用某些 npm 命令时自动生成。

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
  name: 'app-project',
  main: './index.cjs',
  module: './index.mjs'
}
```

如果你只发布一份 ESM 模块化方案，则直接使用 `main` 字段即可。

## `exports` 字段

`exports` 字段可以控制子目录的访问路径。

```json
{
  "exports": {
    ".": "./main.js",
    "./sub/path": "./secondary.js",
    "./prefix/": "./directory/",
    "./prefix/deep/": "./other-directory/",
    "./other-prefix/*": "./yet-another/*/*.js"
  }
}
```

指定 `exports` 字段，将替换 `main` 字段指向的入口文件，且只有在 `exports` 中声明的文件可以被使用。

> 关于 `exports` 字段，可以查阅 webpack 的 [Package exports](https://webpack.docschina.org/guides/package-exports/) 章节。

## `type` 字段

## `funding` 字段

## `workspaces` 字段

## 项目依赖

[`dependencies`](https://github.com/npm/npm/blob/2e3776bf5676bc24fec6239a3420f377fe98acde/doc/files/package.json.md#dependencies) 字段指定了生产环境项目的依赖。当你添加生产环境依赖时，它会自动生成，如：`npm i express`。

```json
{
  "dependencies": {
    "express": "^4.17.1"
  }
}
```

[`devDependencies`](https://github.com/npm/npm/blob/2e3776bf5676bc24fec6239a3420f377fe98acde/doc/files/package.json.md#devdependencies) 字段指定了开发环境项目的依赖。当你添加生产环境依赖时，他会自动生成，如：`npm i eslint -D`

```json
{
  "devDependencies": {
    "eslint": "^6.7.2"
  }
}
```

[`peerDependencies`](https://github.com/npm/npm/blob/2e3776bf5676bc24fec6239a3420f377fe98acde/doc/files/package.json.md#peerdependencies) 早点表示兼容性依赖。如果你的安装的是一个插件，非常适合这种方式。

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

`bundledDependencies` 字段与 `devDependencies` 基本一致。当发布包时，它们也会被打包。

```json
{
  "bundledDependencies": ["super-streams"]
}
```

文档中有详细介绍它们，Stack Overflow 上也有关于这几个字段的详细介绍，请查阅 [What's the difference between dependencies, devDependencies and peerDependencies in npm package.json file?](https://stackoverflow.com/questions/18875674/whats-the-difference-between-dependencies-devdependencies-and-peerdependencies/22004559#22004559) 和 [Advantages of bundledDependencies over normal dependencies in npm](https://stackoverflow.com/questions/11207638/advantages-of-bundleddependencies-over-normal-dependencies-in-npm)。

## `config` 字段

`config` 字段中的键作为 `env` 环境变量公开给脚本。

```json
{
  "name": "node",
  "config": {
    "foo": "hello"
  }
}
```

你可以在应用程序中使用 `config` 字段，当用户执行 `npm run start` 命令时，这个脚本就可以得到值。

```js
console.log(process.env.npm_package_config_foo)
```

你可以使用下面命令改变这个值。

```bash
npm config set node:foo hi
```

## 其他

`homepage` 项目首页的网址。

```json
{
  "homepage": "https://github.com/lio-zero/blog"
}
```

`bugs` 项目的问题追踪系统的 URL 或邮箱地址，这些对遇到软件包问题的人很有帮助。

```json
{
  "bugs": {
    "url": "http://github.com/owner/project/issues",
    "email": "project@hostname.com"
  }
}
```

`bin` 很多的包都会有执行文件需要安装到 `PATH` 中去。这个字段对应的是一个 Map，每个元素对应一个 `{ commandName: fileName }`。这些可执行文件的头部需要加上 `#!/usr/bin/env node`。

```json
{
  "bin": {
    "npm": "./cli.js",
    "command": "./bin/command"
  }
}
```

`private` 是一个布尔值。如果 `private` 为 `true`，可以保证包不会被发布到 npm。这可以防止私有 repositories 不小心被发布出去。

`preferGlobal` 是一个布尔值。如果你的包是个命令行应用程序，需要全局安装，就可以设为 `true`。

```json
{
  "private": true,
  "preferGlobal": true
}
```

`browserslist` 指定项目所支持的浏览器版本。

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

`engines` 字段指定 Node 版本号，也可以指定适用的 `npm` 版本。

```json
{
  "engines": {
    "node": ">= 14.0.0",
    "npm": ">= 6.14.8"
  }
}
```

当本地的 Node 版本与项目的版本不一致，npm 在安装依赖时，将会发出警告，提示你本地的 Node 版本与此项目不符，而 yarn 将直接报错。还有一点很重要，当某些依赖所使用的版本号与项目运行的 Node 版本不一致时，yarn 会也报错，此时项目无法正常运行，可避免意外发生。显示 yarn 这方面做的很好。

`man` 字段用来指定当前项目的 man 文档的位置。

```json
{
  "man": ["./doc/calc.1"]
}
```

`style` 指定供浏览器使用时，样式文件所在的位置。样式文件打包工具 [parcelify](https://github.com/rotundasoftware/parcelify)，通过它知道样式文件的打包位置。

```json
{
  "style": ["./node_modules/tipso/src/tipso.css"]
}
```

`cpu` 指定 CPU 型号。

```json
{
  "cpu": ["x64", "ia32"],
  "cpu": ["!arm", "!mips"]
}
```

`os` 指定项目可以在什么操作系统上运行。

```json
{
  "os": ["darwin", "linux"]
}
```

`files` 当你发布项目时，具体哪些文件会发布上去。如果需要把某些文件不包含在项目中，添加一个 `.npmignore` 文件。这个文件和 `gitignore` 类似。

```json
{
  "files": ["src", "dist/*.js", "types/*.d.ts"]
}
```

`publishConfig` 是一个布尔值。如果你的包是个命令行应用程序，需要全局安装，就可以设为 `true`。

```json
{
  "publishConfig": {
    "access": "public"
  }
}
```

`engineStrick` 是一个布尔值。如果你肯定你的程序只能在制定的 engine 上运行，设置为 true。

还有一些特殊的字段，比如 `prettier`、`unpkg`、`babel`、`jest` 等，他们配合工具使用。

## 根据 `package.json` 内容自动生成 `README.md` 文件

对于任何开源项目来说，最重要的文档就是 README。

这里介绍一个好用的工具 [`readme-md-generator`](https://github.com/kefranabg/readme-md-generator)，它将根据 `package.json` 文件内容自动生成漂亮 README 文件。

`readme-md-generator` 能够读取您的环境（`package.json`、git 配置等），自动创建项目的 `README.md` 文件。

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

## 更多资料

- [npm Docs - package-json](https://docs.npmjs.com/cli/v8/configuring-npm/package-json)
- [package.json](http://caibaojian.com/npm/files/package.json.html)
- [What's what? - Package.json cheatsheet!](https://areknawo.com/whats-what-package-json-cheatsheet/)
