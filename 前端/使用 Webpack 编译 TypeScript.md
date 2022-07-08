# 使用 Webpack 编译 TypeScript

本文将介绍如何使用 Webpack 编译 `.ts` 文件。

## Webpack 配置

Webpack 中的所有内容都从 [Webpack 配置](https://masteringjs.io/tutorials/webpack/config) 开始。`webpack.config.js` 的关键部分是 `module.rules` 选项。这是您告诉 Webpack 在打包之前使用一个特殊的编译器来编译文件的地方。对于 TypeScript，除了 [typescript 模块](https://www.npmjs.com/package/typescript)之外，您还需要 [ts-loader 模块](https://www.npmjs.com/package/ts-loader)。

```bash
npm install typescript ts-loader
```

[`module.rules` 选项](https://webpack.js.org/configuration/module/)是一组规则数组。下面的 `webpack.config.js` 告诉 Webpack 使用该 `ts-loader` 模块编译任何以 `.ts` 结尾的文件。

```js
module.exports = {
  entry: './index.ts',
  output: {
    filename: 'main.js',
    path: `${process.cwd()}/dist`
  },
  module: {
    // 对任何以 .ts 结尾的文件使用 ts-loader
    rules: [
      {
        test: /\.ts$/,
        use: 'ts-loader',
        exclude: /node_modules/
      }
    ]
  },
  // 捆绑 .ts 文件和 .js 文件。
  resolve: {
    extensions: ['.ts', '.js']
  }
}
```

## 编译 TypeScript 文件

下面是 `index.ts` 文件：

```js
const str: string = 'Hello World'

console.log(str)
```

您还需要添加一个 [`tsconfig.json`](https://www.typescriptlang.org/docs/handbook/tsconfig-json.html) 文件，否则 TypeScript 会出错。其规则根据您的项目制定：

```json
{
  "files": ["./index.ts"]
}
```

现在，运行 `node ./dist/main.js`，您将看到 Node 打印出 `Hello World`。

```bash
$ node ./dist/main.js

# Hello World
```

## 更多资料

更加详细内容请查阅 [Webpack 编译 TypeScript 的官方指南](https://webpack.js.org/guides/typescript/)。
