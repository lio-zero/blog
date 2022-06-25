# package.json 详解

当我们创建一个 Node 项目时， 需要创建一个 package.json 文件，描述这个项目所需要的各种模块，以及项目的配置信息（比如名称、版本、许可证等元数据）。

你可以在命令行使用 `npm help package.json` 命令，将跳转到页面，查看这些字段如何使用。下面将根据该文档，进行整理。

## 字段信息

一般，开始一个项目时，我们会在命令行使用 `npm init` 命令生成 `package.json` 文件。

以下是 `package.json` 的一些字段信息：

`name` 项目名

```json
{
  "name": "node"
}
```

`version` 描述项目的当前版本号

```json
{
  "version": "0.1.0"
}
```

`description` 项目的描述

```json
{
  "description": "My package"
}
```

`main` 指定项目的主入口文件

```json
{
  "main": "index.js"
}
```

`author` 项目作者信息，贡献者。它可以有两种写法。`email` 和 `url` 是可选的。

```json
// 方式一：
{
  "name" : "lio-zero",
  "email" : "licroning@163.com",
  "url" : "https://github.com/lio-zero/blog"
}

// 方式二：
{
  "author": "lio-zero <licroning@163.com> (https://github.com/lio-zero/blog)"
}
```

`keywords` 使用相关关键字描述项目

```json
{
  "keywords": ["admin", "node", "node"]
}
```

`license` 许可证（告诉用户可以做什么和不能做什么，常见：MIT、BSD-3-Clause）

```json
{
  "keywords": "MIT"
}
```

`scripts` 指定运行脚本命令的 npm 命令行缩写，比如 start 指定了运行 `npm run start` 时，所要执行的命令。

```json
{
  "scripts": {
    "start": "node ./bin/xxx"
  }
}
```

`repository` 字段用于指定代码存放的位置。

```json
{
  "repository": {
    "type": "git",
    "url": "这里写上项目在 github 上的地址"
  }
}
```

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

使用 `npm init -y` 默认生成的 `package.json` 文件会少一个 `repository` 字段，需要的话可以手动添加上去。

接下来我们将在这基础上介绍一些其他字段，他们都是可选的，也会在您使用某些 npm 命令时自动生成。

## 依赖

[`dependencies`](https://github.com/npm/npm/blob/2e3776bf5676bc24fec6239a3420f377fe98acde/doc/files/package.json.md#dependencies) 字段指定了生产环境项目的依赖。当你添加生产环境依赖时，他会自动生成，如：`npm i express`

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

[`peerDependencies`](https://github.com/npm/npm/blob/2e3776bf5676bc24fec6239a3420f377fe98acde/doc/files/package.json.md#peerdependencies) 兼容性依赖。如果你的包是插件，适合这种方式。

```json
{
  "peerDependencies": {
    "tea": "2.x"
  }
}
```

[`optionalDependencies`](https://github.com/npm/npm/blob/2e3776bf5676bc24fec6239a3420f377fe98acde/doc/files/package.json.md#optionaldependencies) 如果你想在某些依赖即使没有找到，或则安装失败的情况下，npm 都继续执行。那么这些依赖适合放在这里。

```json
{
  "optionalDependencies": {}
}
```

`bundledDependencies` 发布包时捆绑的包名数组

```json
{
  "bundledDependencies": ["renderized", "super-streams"]
}
```

## 配置

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

## lint-staged

在代码提交之前，进行代码规则检查能够确保进入 git 库的代码都是符合代码规则的。但是整个项目上运行 lint 速度会很慢，lint-staged 能够让 lint 只检测暂存区的文件，所以速度很快。

**安装与配置**：`husky` 和 `lint-staged`

```bash
npm i husky lint-staged -D
```

`package.json` 中配置：

```json
{
  "husky": {
    "hooks": {
      "pre-commit": "lint-staged"
    }
  },
  "lint-staged": {
    "*.js": "eslint --fix"
  }
}
```

`git commit` 时触发 `pre-commit` 钩子，运行 `lint-staged` 命令，对 `*.js` 执行 `eslint` 命令。`eslint` 要提前配置好。

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

`bin` 很多的包都会有执行文件需要安装到 PATH 中去。这个字段对应的是一个 Map，每个元素对应一个**{ 命令名：文件名 }**。这些可执行文件的头部需要加上 `#!/usr/bin/env node`。

```json
{
  "bin": {
    "npm": "./cli.js",
    "command": "./bin/command"
  }
}
```

`private` 是一个布尔值。如果 private 为 true，可以保证包不会被发布到 npm。这可以防止私有 repositories 不小心被发布出去。

`preferGlobal` 是一个布尔值。如果你的包是个命令行应用程序，需要全局安装，就可以设为 true。

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

`engines` 字段指明了该项目运行的平台，比如 Node 的某个版本或者浏览器，也可以指定适用的 `npm` 版本。

```json
{
  "engines": {
    "node": ">= 12.16.2",
    "npm": ">= 6.14.8"
  }
}
```

`man` 用来指定当前项目的 man 文档的位置。

```json
{
  "man": ["./doc/calc.1"]
}
```

`style` 指定供浏览器使用时，样式文件所在的位置。样式文件打包工具 parcelify，通过它知道样式文件的打包位置。

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

`publishConfig` 是一个布尔值。如果你的包是个命令行应用程序，需要全局安装，就可以设为 true。

`engineStrick` 是一个布尔值。如果你肯定你的程序只能在制定的 engine 上运行，设置为 true。

还有一些特殊的字段，比如 `prettier`、`unpkg`、`babel`、`jest` 等，他们配合工具使用。

## 根据 package.json 内容自动生成 README.md 文件

对于任何开源项目来说，最重要的文档就是 README。

这里介绍一个好用的工具 [`readme-md-generator`](https://github.com/lio-zero/readme-md-generator)，它将根据 `package.json` 文件内容自动生成漂亮 README 文件

`readme-md-generator` 能够读取您的环境（`package.json`，git 配置等），创建项目的 README 文件。

以下是该模式提供的一个示例演示：

![使用示例](https://upload-images.jianshu.io/upload_images/18281896-f3c63011175001bf.gif?imageMogr2/auto-orient/strip)

### 基本配置

确保已安装 [npx](https://www.npmjs.com/package/npx)（自 npm 以来默认 `npx` `5.2.0`)

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

> 详细内容可以查阅文档，例如可以单独定制模板等。

## 更多资料

- [npm Docs - package-json](https://docs.npmjs.com/cli/v8/configuring-npm/package-json)
- [package.json](http://caibaojian.com/npm/files/package.json.html)
- [What's the difference between dependencies, devDependencies and peerDependencies in npm package.json file?](https://stackoverflow.com/questions/18875674/whats-the-difference-between-dependencies-devdependencies-and-peerdependencies/22004559#22004559)
- [What's what? - Package.json cheatsheet!](https://areknawo.com/whats-what-package-json-cheatsheet/)
