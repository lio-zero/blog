# esbuild 入门

[esbuild](https://esbuild.github.io/) 是一个快速的 JavaScript 打包器，主要目标是带来构建工具性能的新时代，并在此过程中创建易于使用的现代捆绑器。

esbuild 是一个用 Go 编写的快速而简单的 JavaScript 捆绑包，其主要特点有：

- 极速，无需缓存
- ES6 和 CommonJS 模块
- ES6 模块的 Tree Shaking
- JavaScript 和 Go 的 [API](https://esbuild.github.io/api/)
- [TypeScript](https://esbuild.github.io/content-types/#typescript) 和 [JSX](https://esbuild.github.io/content-types/#jsx) 语法
- [sourcemap](https://esbuild.github.io/api/#sourcemap)
- [minify](https://esbuild.github.io/api/#minify)
- [plugins](https://esbuild.github.io/plugins/)

以上是 esbuild 官方对该项目的一些介绍。

本文将介绍 esbuild 的一些基本的用法，如何捆绑 JavaScript 应用程序，包括 TypeScript、图像文件、CSS 等。

## 安装 esbuild

首先，使用 npm 安装 esbuild：

```bash
$ npm i -g esbuild
```

然后，您可以通过调用 esbuild 命令来验证是否安装成功：

```bash
$ esbuild --version
# 0.14.38
```

如果您不想在全局范围内安装 esbuild，也可以这样做：

```git
$ npm i esbuild
```

但您必须使用完整路径调用 esbuild：

```git
$ ./node_modules/.bin/esbuild --version
# 0.14.38
```

## 使用 esbuild 捆绑 TypeScript

创建一个 `main.ts` 文件，其中有如下内容：

```ts
let message: string = 'Hello esbuild!'
console.log(message)
```

通过 CLI 捆绑 TypeScript 代码：

```bash
$ esbuild main.ts --outfile=output.js --bundle --loader:.ts=ts
  output.js  87b

Done in 4ms
```

esbuild 命令接受 `main.ts` 作为入口，它是应用程序启动的地方。

然后，提供 `outfile` 选项作为定义输出文件的方法。如果不提供此选项，esbuild 会将结果发送到 `stdout`。

`loader` 选项是用于加载 TypeScript 文件扩展名的选项。但是，您可以忽略此选项，因为 esbuild 可以根据文件扩展名决定使用哪个 `loader`。

通过 `bundle` 选项，esbuild 将把所有依赖项内联到 `output.js` 文件中。

运行命令后，将捆绑到 `output.js` 文件，内容如下：

```js
;(() => {
  // main.ts
  var message = 'Hello esbuild!'
  console.log(message)
})()
```

我们来看一下不使用 `bundle` 选项的情况。

我们更改 `main.ts` 文件的内容如下：

```ts
import { sayHello } from './lib'

SayHello()
```

在创建 `lib.ts` 文件，其中编写一个 `sayHello` 并导出：

```js
export function sayHello() {
  console.log('Hello esbuild!')
}
```

如果不使用 `bundle` 选项，esbuild 只会在结果中导入依赖项：

```bash
$ esbuild main.ts
import { sayHello } from "./lib";
sayHello();
```

但如果使用 `bundle` 选项，esbuild 将在结果中内联 `lib.ts` 的内容：

```bash
$ esbuild main.ts --bundle
(() => {
  // lib.ts
  function sayHello() {
    console.log("Hello esbuild!");
  }

  // main.ts
  sayHello()
})();
```

使用 `bundle` 选项，可以将所有代码打包到一个文件中。换句话说，两个文件变成一个文件。

## 使用 esbuild 捆绑 React

使用 esbuild 集成 React 到项目中及极其简单。

首先，使用 npm 安装 React 库：

```bash
npm i react react - dom
```

然后创建一个名为 `App.js` 的文件，添加以下内容：

```js
import React from 'react'
import ReactDOM from 'react-dom'

function App() {
  return <div>Hello esbuild!</div>
}

ReactDOM.render(<App />, document.getElementById('root'))
```

创建一个 HTML 文件，以便 React 可以将您的应用程序渲染到具有 ID 为 `root` 的 `div` 中。将以下代码添加到 `index.html` 文件中：

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <meta
      name="viewport"
      content="width=device-width, initial-scale=1, shrink-to-fit=no"
    />
    <title>Hello esbuild!</title>
  </head>

  <body>
    <div id="root"></div>
    <script src="AppBundle.js"></script>
  </body>
</html>
```

在 HTML 文件中，我们使用 `AppBundle.js` 这是捆绑的 JavaScript 文件的名称。

```bash
esbuild App.js --bundle --outfile=AppBundle.js --loader:.js=jsx
```

我们使用 `bundle` 选项绑定 JavaScript 文件。然后，使用 `outfile` 选项为输出文件指定所需的名称。

最后一个选项 `loader` 实际上不是可选的。告诉 esbuild 对带有 `.js` 扩展名的文件使用 JSX 加载器，因为 JSX 语法在 `App.js` 中。如果不使用 JSX 加载器，esbuild 将抛出一个错误。如果输入文件的扩展名是 `.jsx` 而不是 `.js`，可以省略 `loader` 选项。因此，如果你将 JavaScript 文件命名为 `App.jsx`，那么你可以忽略 `loader` 选项。

现在您有了 `AppBundle.js`，让我们打开 `index.html` 来检查您的绑定过程是否正常工作。

这里我们需要通过 server 的方式打开 `index.html`，推荐 VS Code 插件 [Live Server](https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer)，也可以使用 [`http-server`](https://www.npmjs.com/package/http-server) 模块，我们也会在下面介绍到 esbuild 的 `serve` 模式，来开启一个 server。

![Hello esbuild](https://upload-images.jianshu.io/upload_images/18281896-6929781c593a7f71.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 将 CSS 与 esbuild 捆绑

假设我们的 `color.css` 文件有以下内容：

```css
.item {
  color: plum;
}
```

创建 `style.css` 导入 `color.css`：

```css
@import 'color.css';

p {
  font-weight: bold;
}
```

要捆绑这两个 CSS 文件，可以运行如下 esbuild 命令：

```bash
$ esbuild style.css --outfile=out.css --bundle

  out.css  85b

Done in 5ms
```

查看 `out.css` 文件：

```css
/* color.css */
.item {
  color: plum;
}

/* style.css */
p {
  font-weight: bold;
}
```

现在，您只需要在 HTML 文件中包含该文件即可。

如果您想要压缩文件，可以使用 `--minify` 选项：

```bash
$ esbuild style.css --outfile=out.css --bundle --minify

  out.css  36b

Done in 7ms
```

压缩后的内容如下：

```css
.item {
  color: plum;
}
p {
  font-weight: 700;
}
```

## 捆绑图像

您还可以将图像与 esbuild 捆绑在一起。

捆绑图像有两种选择：

- 将图像作为外部文件加载到 JavaScript 文件中
- 将图像作为 Base64 编码的 Data URL 嵌入到 JavaScript 文件中

让我们看看两者的区别。

创建一个 `index.html` 文件，并添加以下内容：

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <meta
      name="viewport"
      content="width=device-width, initial-scale=1, shrink-to-fit=no"
    />
    <title>Hello, esbuild!</title>
  </head>

  <body>
    <div id="root">
      <div>
        <img id="image_png" />
      </div>
      <div>
        <img id="image_jpg" />
      </div>
    </div>
    <script src="out_main.js"></script>
  </body>
</html>
```

然后，创建一个 `main.js` 文件，并添加以下内容：

```js
import png_url from './example.png'
const png_image = document.getElementById('image_png')
png_image.src = png_url

import jpg_url from './baidu.jpg'
const jpg_image = document.getElementById('image_jpg')
jpg_image.src = jpg_url
```

这里，我们使用 JavaScript 文件中的 `import` 语句加载图像。与绑定 CSS 文件不同，您不直接绑定图像，而是通过绑定引用图像的 JavaScript 文件来绑定图像。

现在，打包 JavaScript 文件：

```bash
$ esbuild main.js --bundle --loader:.png=dataurl --loader:.jpg=file --outfile=out_main.js

  out_main.js         102.7kb
  baidu-YKEFL4XA.jpg   76.8kb

Done in 15ms
```

这里使用了两个 `loader`。`.png` 扩展使用 `dataurl` 和 `.jpg` 扩展使用 `file` 。使用 `file` 加载器的将会生成一个 `baidu-YKEFL4XA.jpg` 的新文件。

查看生成的 `out_main.js` 文件，内容如下：

```js
;(() => {
  // example.png
  var example_default = 'data:image/png;base64,iVBORw0K...'

  // baidu.jpg
  var baidu_default = './baidu-YKEFL4XA.jpg'

  // main.js
  var png_image = document.getElementById('image_png')
  png_image.src = example_default
  var jpg_image = document.getElementById('image_jpg')
  jpg_image.src = baidu_default
})()
```

正如您所看到的，第一张图片使用 Based64 编码的 Data URL 格式。第二个图像使用文件路径格式。对于第二张图片，还有一个名为 `baidu-YKEFL4XA.jpg` 的外部文件。

最后，通过 server 的方式打开 `index.html`，查看效果。

## 使用插件

esbuild 并不是一个完整的捆绑解决方案。它默认支持 React、CSS 和图像，但不支持 SASS 预处理等器。如果你想捆绑 SASS 文件，则需要安装一个 esbuild 插件。

> [esbuild 插件列表](https://github.com/esbuild/community-plugins)

有几个插件可以捆绑 SASS 文件。这里使用我们 `esbuild-plugin-sass`。

使用 npm 安装插件，如下所示：

```bash
npm install esbuild-plugin-sass -D
```

我们在 `style.scss` 文件内编写一些简单的样式：

```scss
// style.scss
$font: Roboto;
$color: plum;

#root {
  font: 1.2em $font;
  color: $color;
}
```

要使用 `esbuild-plugin-sass` 插件，需要使用 esbuild 的 `build` API。

在 `main.js` 添加以下内容：

```js
const sassPlugin = require('esbuild-plugin-sass')

require('esbuild')
  .build({
    entryPoints: ['style.scss'],
    outfile: 'bundle.css',
    bundle: true,
    plugins: [sassPlugin()]
  })
  .then(() => console.log('Done'))
  .catch(() => process.exit(1))
```

我们使用 `plugins` 来引用 SASS 插件，`entryPoints` 引用是我们编写的 scss 文件，其返回的是一个 Promise，成功后我们输出 `Done`。

执行 `main.js` 文件：

```bash
$ node main.js
Done
```

您可以打开 `bundle.css` 文件检查输出结果：

```css
/* ../../temp/tmp-10260-4kl4Lq4Efuiv/esbuild-test/style.css */
#root {
  font: 1.2em Roboto;
  color: plum;
}
```

## watch 模式

esbuild 的 `watch` 模式会监听入口文件，并自动打包。

我们监听 `main.ts` 文件，如果其中内容存在变动，将自动重新打包：

```js
// build.js
require('esbuild')
  .build({
    entryPoints: ['main.ts'],
    outfile: 'output.js',
    bundle: true,
    loader: { '.ts': 'ts' },
    watch: true
  })
  .then(() => console.log('Done'))
  .catch(() => process.exit(1))
```

`main.ts` 文件的内容如下：

```ts
let message: string = 'Hello esbuild!'
console.log(message)
```

现在，我们执行 `build.js` 文件：

```bash
node build.js
```

你可能会注意，构建的过程并未中断，仍处于活动状态，esbuild 会持续监听 `mian.ts` 文件的变动。

我们先查看 `output.js` 文件：

```js
;(() => {
  // main.ts
  var message = 'Hello esbuild!'
  console.log(message)
})()
```

回到 `main.ts` 文件，修改一些内容：

```ts
let message: string = 'Hello World!'
console.log(message)
```

在查看 `output.js`：

```js
;(() => {
  // main.ts
  var message = 'Hello World!'
  console.log(message)
})()
```

可以看到输出的文件会自动更新。

`watch` 可以监视文件系统，以便 esbuild 在检测到文件更改时可以捆绑输入文件。

## serve 模式

还有另一种自动绑定文件的方法，称为 `serve` 模式。这意味着您要启动一台服务器来为输出文件提供服务。如果有人从浏览器请求输出文件，如果文件已更改，服务器将自动绑定输入文件。

以下是一个 `index.html` 内容：

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Hello esbuild!</title>
  </head>
  <body>
    <script src="output.js"></script>
  </body>
</html>
```

其中引入了 `main.ts` 捆绑后输出的 `output.js`，`main.ts` 文件内容如下：

```ts
let message: string = 'Hello esbuild!'
console.log(message)
```

我们使用 `serve` 模式捆绑文件：

```bash
$ esbuild main.ts --outfile=output.js --bundle --loader:.ts=ts --serve=localhost:8000 --servedir=.

> Local: http://127.0.0.1:8000/
```

`serve` 选项用于定义服务器和端口。`servedir` 选项定义服务器服务的目录。

访问 `http://127.0.0.1:8000`，打开控制台查看输入内容。然后尝试着修改 `main.ts` 的内容，刷新页面查看修改后的内容。

## 最后

本文简单介绍 esbuild 的一些基本用法，例如 sourcemap 等我们是没讲到的，详细内容可以查看[文档](https://esbuild.github.io/)，后面学习过程中也会进行总结。
