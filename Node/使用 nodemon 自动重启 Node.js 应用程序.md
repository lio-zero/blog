# 使用 nodemon 自动重启 Node.js 应用程序

[nodemon](http://nodemon.io/) 是一种工具，可在检测到目录中的文件更改时通过自动重新启动节点应用程序来帮助开发基于 node.js 的应用程序。

## nodemon 特性

- 自动重新启动应用程序。
- 检测要监视的默认文件扩展名。
- 默认支持 node，但易于运行任何可执行文件，如 python、ruby、make 等。
- 忽略特定的文件或目录。
- 监视特定目录。
- 使用服务器应用程序或一次性运行实用程序和 REPL。
- 可通过 Node require 语句编写脚本。
- 开源，在 [github](https://github.com/remy/nodemon/) 上可用。

## 安装

全局安装：

```bash
npm i -g nodemon
```

本地安装：

```bash
npm i -D nodemon
```

注意：本地安装需要在 `package.json` 文件的 script 脚本中指定要需要执行的命令

```json
{
  "script": {
    "dev": "nodemon app.js"
  }
}
```

使用 `npm dev` 运行

## 使用

`nodemon` 一般只在开发时使用，它最大的长处在于 watch 功能，一旦文件发生变化，就自动重启进程。

```bash
# 默认监视当前目录的文件变化
$ nodemon app.js

# 指定主机和端口作为参数，表示在本地 3697 端口启动 node 服务
$ nodemon app.js localhost 3697
```

## 参数

您可以使用以下命令查看所有可用选项：

```bash
nodemon --help
```

以下是它们的详细解释。

### watch

监控的文件夹路径或者文件路径。

`watch` 可以监控多个目录，默认值：`'*.*'`。默认情况下，nodemon 监控当前工作目录。如果您想要控制该选项，请使用该选项添加特定路径：`--watch`

```bash
# 监控指定文件夹或者文件变化
$ nodemon --watch app --watch libs app.js
```

现在，`nodemon` 只有在 `./app` 目录或 `./libs` 目录下文件发生变化时才会重新启动。

### ext

默认监听：`"js, mjs, json"`

监控指定后缀名的文件，用空格间隔。

`ext` 监听指定文件扩展名的文件。默认情况下，nodemon 查找扩展名为 `.js`，`.mjs`，`.coffee`，`.litcoffee`，和 `.json` 的文件。

使用 `-e` 或 `--ext` 指定监听的文件扩展名，如下所示：

```bash
nodemon -e js,pug
```

**优先级**：nodemon 会先读取 `watch` 里面需要监听的文件或文件路径，再从文件中选择监控 `ext` 中指定的后缀名，最后去掉从 `ignore` 中指定的忽略文件或文件路径。

### exec

`exec` 执行项。若设定了执行项，`nodemon` 将执行程序而不是 JavaScript 脚本。

```bash
nodemon --exec "python -v" ./app.py
```

### ignore

`ignore` 忽略项（包括文件、目录或文件名通配符匹配）。

注意，默认情况下，nodemon 会忽略 `.git`，`node_modules`，`bower_components`，`.nyc_output`，`coverage` 和 `.sass-cache` 目录，并添加你的忽略模式到列表中。将 `ignore` 置空并不能取消忽略。

```bash
# 忽略特定的文件
$ nodemon --ignore lib/app.js

# 忽略多个目录
$ nodemon --ignore lib/ --ignore tests/

# 使用通配符匹配（但是一定要引用引号），* 表示该文件夹下的所有后缀为 .js 的文件
$ nodemon --ignore 'lib/*.js'
```

### execMap

`execMap` 设置运行服务的后缀名与对应的命令。

```json
{
  "execMap": {
    "js": "npm -v"
  }
}
```

可以用来定义默认可执行文件，如果您使用的语言在默认情况下不受 Node 支持，则此应用特别有用。

```json
{
  "execMap": {
    "pl": "perl"
  }
}
```

现在运行以下命令，nodemon 将知道将其 `perl` 用作可执行文件：

```bash
nodemon app.pl
```

设置运行服务的后缀名与对应的命令

### delay

`delay` 延迟重启时间（毫秒）。延迟重启类似于 JavaScript 函数中的函数节流，只在最后一次更改的文件往后延迟重启，以避免了短时间多次重启。

```bash
nodemon --delay 10 app.js
nodemon --delay 2.5 app.js
nodemon --delay 2500ms app.js
```

### verbose

`verbose` 设置日志输出模式，true 详细模式

```json
{
  "verbose": true
}
```

![verbose](https://upload-images.jianshu.io/upload_images/18281896-1a91aa63832544b3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### colours

`colours` 默认为 `true`，输出信息颜色标示。

```json
{
  "colours": "false"
}
```

![colours](https://upload-images.jianshu.io/upload_images/18281896-799e140182bdc11d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### events

`events` 表示 nodemon 运行到某些状态时的一些触发事件，总共有五个状态：

- `start`：子进程（即监控的应用）启动
- `crash`：子进程崩溃，不会触发 exit
- `exit`：子进程完全退出，不是非正常的崩溃
- `restart`：子进程重启
- `config:update`：nodemon 的 config 文件改变

参考：[使用 nodemon 作为子进程](https://github.com/remy/nodemon/blob/master/doc/events.md#Using_nodemon_as_child_process)

### restartable

`restartable` 设置重启模式。重启的命令，默认是 rs，可以改成你自己喜欢的字符串。

```json
{
  "restartable": "nv"
}
```

在运行的情况下输入 `rs` 即可

![restartable](https://upload-images.jianshu.io/upload_images/18281896-9ab0e9c811f2de24.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### env

`env` 运行环境

```json
{
  "env": {
    "NODE_ENV": "development", // 开发环境
    "PORT": "3000" // 端口号
  }
}
```

## 配置文件

你可以在命令行中添加参数选项以支持某种功能，也可以使用本地和全局配置文件。可以使用 `--config` 选项指定备用本地配置文件。

```json
// nodemon.json
{
  "restartable": "nv",
  "delay": 1000,
  "colours": false,
  "verbose": true,
  "ignore": ["./src"],
  "watch": ["app.js", "src"],
  "events": {
    "restart": "osascript -e 'display notification \"app restarted\" with title \"nodemon\"'"
  },
  "execMap": {
    "js": "npm -v"
  },
  "ext": "js, json",
  "env": {
    "NODE_ENV": "development",
    "PORT": "3000"
  }
}
```

注意，使用 `execMap` 代替 `--exec`。`execMap` 允许您为某些文件扩展名指定二进制文件。

如果您不想将 `nodemon.json` 配置文件添加到项目中，你还可以在 `package.json` 中使用 `nodemonConfig` 字段进行配置，这时独立配置文件将被忽略。

```json
{
  "name": "demo",
  "nodemonConfig": {
    "ignore": ["node_modules", "dist"], // 忽略 node_modules 和 dist 文件
    "delay": "2500",
    "watch": ["app.js", "src"]
  }
}
```

**优先级**：本地配置文件 -> nodemonConfig -> 全局配置文件。命令行中指定的参数选项会被本地配置文件覆盖，而在 `package.json` 中配置的会被命令行覆盖。

每次修改配置文件修改完记得重启一下。

## nodemon 的默认配置文件

nodemon 的[默认配置文件](https://github.com/remy/nodemon/blob/master/lib/config/defaults.js)

```js
var ignoreRoot = require('ignore-by-default').directories()

// 默认选项配置选项
module.exports = {
  restartable: 'rs',
  colours: true,
  execMap: {
    py: 'python',
    rb: 'ruby',
    ts: 'ts-node'
    // 更多的可以在这里添加，如 ls:lsc - 但请确保它是交叉的与 linux、mac 和 windows 兼容，或使 default.js 为基于 node 的实用程序动态附加 .cmd
  },
  watch: ['*.*'],
  stdin: true,
  runOnChangeOnly: false, // 为 true 时运行 nodemon xxx 项目不会启动，只保持对文件的监控，当监控的文件有修改并保存时才会启动应用，其他没有影响。默认是 false 即一开始就启动应用并监控文件改动。
  verbose: false,
  signal: 'SIGUSR2',
  stdout: true, // 这个是关于标准输入输出的设置，上文提到 nodemon.json 文件中的 events 字段可以为状态设置标准输入输出语句，如果这里设置了 false，标准输入输出语句就会失效。
  watchOptions: {}
}
```

## 更多

- [使用 nodemon 作为模块](https://github.com/remy/nodemon/blob/master/doc/requireable.md)
- [gulp-nodemon](https://github.com/JacksonGariety/gulp-nodemon) 插件，将 nodemon 与项目的其余 Gulp 工作流程集成在一起。
- [grunt-nodemon](https://github.com/ChrisWren/grunt-nodemon) 插件，将 nodemon 与项目的其余 Grunt 工作流程集成在一起。
- [常问问题](https://github.com/remy/nodemon/blob/master/faq.md#overriding-the-underlying-default-ignore-rules)

<!-- https://www.digitalocean.com/community/tutorials/workflow-nodemon -->
