# Node.js 读取环境变量

## 什么是环境变量？

在软件开发中，“环境”是程序或进程运行的环境。

[环境变量](https://en.wikipedia.org/wiki/Environment_variable)是以某种方式调整环境（进程）的值。

例如，考虑一些变量，在本地开发和在生产中运行时，您希望它们的值不同。虽然可以通过在代码中编写条件变量来实现这一点，但使用环境变量更容易（也是更好的做法）。

下面是一个 Node.js 应用程序中的示例：

```js
// bad
const env = isProduction ? 'production' : 'development'

// good
const env = process.env.NODE_ENV
```

Node 的 `process` 核心模块提供了 `env` 属性，该属性承载了流程启动时设置的所有环境变量。

> **注意**：`process` 是一个全局对象，不需要额外的 `require` 导入。

## 设置环境变量

可以通过多种方式设置环境变量，这些方式取决于进程运行的系统。该操作通常在命令行上执行，但许多编程语言都有机制和库，可以让这个过程更容易。

还有，像 [Vercel](https://vercel.com/) 和 [Netlify](https://www.netlify.com/)（以及其他）这样的现代部署平台，它们提供了一些接口让我们能够添加环境变量。

## 存储敏感信息

因为环境变量特定于环境而非代码，所以它们是进程存储敏感值的好方法。

## 何时使用环境变量

作为新开发人员，可能很难理解环境变量的值以及它们何时有意义。

我发现环境变量在两种情况下最有用：

- 特定于环境的值
- 敏感数据

让我们先看看这些场景中的每一个，以帮助推导出使用环境变量的值。

### 特定于环境的值

您的大多数项目都有多个环境。至少，您将拥有一个开发环境（用于本地工作）和一个生产环境。但项目通常会有额外的 `test`、`qa`、`staging` 等环境。

在环境之间更改值时的新手路线通常如下所示（以 JS 代码为例）：

```js
if (isProduction) {
  const my_var = 'This Value'
} else {
  const my_var = 'That Value'
}
```

看起来不够优雅。它是重复的（不是 [DRY](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself)），它只解决两个环境的上下文值。当您添加新的环境时，您都必须在具有此类逻辑的任何地方添加新行。

如果使用环境变量，可以将这五行减为一行：

```js
const my_var = MY_VALUE
```

使用环境变量管理特定于环境的值使您能够保持代码清洁，并管理其他环境中的更改。

### 敏感数据

敏感数据（密码、API 密钥、令牌等），不应写入代码中。即使您的 Git 主机（[GitHub](https://github.com/)、[Bitbucket](https://bitbucket.org/) 或 [GitLab](https://about.gitlab.com/) 等）上有一个私有存储库，也是如此。当您提交一个敏感值并将其推送到远程主机时，您又添加了一个敏感值可能被盗的位置。

使用环境变量存储敏感数据意味着该变量只存在于它被使用的地方，从而尽可能有效地降低了暴露它的风险。

而且，当您拥有一个可靠的系统来存储敏感数据时，您就不必太担心这些值的敏感程度。如果对某个值是否敏感有任何疑问，您应该自动默认使用环境变量，而不是将其插入代码中。

## 使用一些工具来管理环境变量

我所使用过的一些好的工具：

- [direnv](https://github.com/direnv/direnv)
- [dotenv](https://github.com/motdotla/dotenv)
- [cross-env](https://github.com/kentcdodds/cross-env)

以 `dotenv` 为例：

可以在 `.env` 文件中写入环境变量（您应该将该文件添加到 `.gitignore` 中，以避免推送到 GitHub），然后，在主文件的开头添加：

```js
require('dotenv').config()
```

通过这种方式，您可以避免在 Node 命令之前在命令行中列出环境变量，这些变量将自动提取。

它们的详细用法可以查阅各自文档。
