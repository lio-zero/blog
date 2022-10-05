# Parcel 入门

[Parcel](https://parceljs.org/) 是一个面向 web 的零配置构建工具。它将出色的开箱即用的开发体验与可伸缩的体系结构相结合，可以将您的项目从刚开始的阶段发展到大规模的生产应用。

它和 webpack 属于同一个工具类别，但有着不同的价值主张。

其主要特点包括：

- 资源捆绑（JS、CSS、HTML、图像）
- 零配置代码拆分
- 使用 Babel、PostCSS 和 PostHTML 进行自动转换
- 热模块替换
- 代码诊断
- 开发服务器
- 缓存和并行处理以加快构建速度
- etc

Parcel 承诺无需任何配置即可完成所有这些工作，并且速度也很快。

## 基本用法

全局安装：

```bash
npm i -g parcel-bundler
```

使用：

```bash
parcel watch index.html
```

它将开始扫描 HTML 页面源中的资源，并用输出文件替换它处理的任何引用。

你也可以将 Parcel 指向一个 JS 文件：

```bash
parcel watch entry.js
```

## 开发服务器

Parcel 附带了一个内置的开发服务器，因此您无需设置。您可以从以下方式开始：

```bash
parcel index.html
```

## 打包

当你对你的应用感到满意，并且你想创建一个可用于生产环境的捆绑包时，运行：

```bash
parcel build index.html
```

生产捆绑包禁用热模块更新，不监视您的更改，一旦捆绑完成，它就会存在，并使用压缩器压缩代码。

它还自动将 `NODE_ENV` 变量应用到生产中，使库生成生产环境代码。

## 资源

### JavaScript

Parcel 支持 ES Modules（`import...`）和 CommonJS（`require...`）。

它使用动态导入执行自动[代码拆分](https://parceljs.org/code_splitting.html)。

动态导入返回一个 Promise，而不是在应用启动时加载所有依赖项，我们可以要求浏览器加载它们，但仅在应用的某些部分就绪时执行。

或者，我们可以要求浏览器仅在需要时才加载它们，例如当用户单击特定链接时。

### CSS

Parcel 支持纯 CSS、SASS、Less 和 Stylus。

您可以使用 [CSS Modules](https://github.com/css-modules/css-modules) 编写 CSS 。

### 转换

当 Parcel 发现您有以下配置之一：

- Babel（`.babelrc`）
- PostCSS（`.postcssrc`）
- PostHTML（`.posthtmlrc`）

它将在捆绑过程中自动使用它。

### 热模块更新

热模块替换（HMR）是构建应用时的一项有用功能。当我们保存 CSS 或 JavaScript 的新副本时，HMR 负责更新浏览器中的模块，而不刷新整个应用。

## Parcel 与 webpack 的对比

webpack 可能对一些新开发人员来说，相当困难，它允许你做很多事情，但也意味着我们需要仔细配置它，使其完全满足我们的需求。

有时 `webpack.config.js` 文件增长到数百行，我们将其复制/粘贴到下一个项目中，如果我们需要修改它，这是一个痛苦的过程。

Parcel 承诺我们可以做很多 webpack 所做的事情，但根本不需要任何配置，依赖于约定而不是配置。

从 webpack 4 开始，将引入零配置模式，这主要是为了响应 Parcel 的成功，它带有一些约定，而不是总是需要配置。

Parcel 不需要任何配置即可快速开发小型应用项目，且只需一些简单的配置即可将小型项目扩展到大型应用。

## 最后

到此结束，更多详细内容请查阅[官方文档](https://parceljs.org/docs/)。
