# 如何在 Node.js 应用程序中使用 ESLint

[ESLint](https://eslint.org/) 是一个开源 JavaScript lint 实用程序，它可以帮助您克服开发人员的错误，因为 JavaScript 是弱类型的语言。

JavaScript 社区中有很多选项，比如 [JSLint](http://jslint.com/)、[JSHint](https://jshint.com/) 和 [JSCS](https://jscs-dev.github.io/)（未维护，已与 ESLint 合并），包括今天我们要讲的 ESLint。

ESLint 旨在使所有规则完全可插入。这是它产生的主要原因之一。它允许开发人员创建自己的 lint 规则。[ESLint 官方指南](http://eslint.org/docs/user-guide)中提供的每个规则都是独立的规则，开发人员可以在任何时候决定是否使用特定的规则。

## 安装

对于项目目录的本地安装：

```bash
npm i eslint -D
```

对于工作系统中的全局安装：

```bash
npm i eslint -g
```

现在可以通过终端中的 `eslint` 命令使用 ESLint。

## 配置

最简单的配置方法是设置一个 `.eslintrc` JSON 文件，其中可以描述所有的 linting 规则。

`.eslintrc` 的一个示例：

```json
{
  "env": {
    "node": true,
    "browser": true
  },
  "rules": {
    "no-console": 0,
    "space-infix-ops": "error",
    "quotes": [
      "error",
      "single",
      {
        "avoidEscape": true,
        "allowTemplateLiterals": true
      }
    ],
    "space-before-blocks": ["error", "always"],
    "semi": ["error", "never"]
  },
  "plugins": []
}
```

**主要字段：**

- `parse` — 指定解析器
- `parserOptions` — 指定解析器选项
- `env` — 指定脚本的运行环境
- `root` — 为 `true` 时，停止向上查找父级目录中的配置文件
- `globals` — 脚本在执行期间访问的额外的全局变量
- `rules` — 在此处添加您的自定义规则

如果全局安装了 eslint，我们还可以使用以下命令生成配置文件：

```bash
eslint --init
```

在其他情况下，如果您已在本地将其安装到项目中，则需要在终端中输入：

```bash
./node_modules/.bin/eslint --init
```

在这两种情况下，都会提示您生成 `.eslintrc` 文件的一组基本规则。

![carbon.png](https://upload-images.jianshu.io/upload_images/18281896-7fde98f136cb001c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

上述提示后生成的文件示例：

```json
{
  "env": {
    "browser": true,
    "commonjs": true,
    "es2021": true
  },
  "extends": "eslint:recommended",
  "parserOptions": {
    "ecmaVersion": 12
  },
  "rules": {
    "indent": ["error", "tab"],
    "linebreak-style": ["error", "windows"],
    "quotes": ["error", "single"],
    "semi": ["error", "never"]
  }
}
```

> 有关配置的详细信息，请[阅读此处](http://eslint.org/docs/user-guide/configuring)。

为了方便运行，我们打开项目的 `package.json`，在 `scripts` 字段里面添加以下脚本：

```json
{
  "scripts": {
    "lint": "eslint **/*.js",
    "lint-html": "eslint **/*.js -f html -o ./reports/lint-results.html",
    "lint-fix": "eslint --fix **/*.js"
  }
}
```

我们将该规则应用于以下文件：

```js
var a = 1
console.log(1)
```

执行 `npm run lint` 后将出现以下信息：

![npm run lint](https://upload-images.jianshu.io/upload_images/18281896-1eda7c004ce32f4d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

ESLint 提示已经很明显了：3 个错误。第一行和第二行的最后又额外的分号错误，`a` 被赋值但从未使用。

并且提示使用 `--fix` 选项修复错误和警告，有 2 个错误是可以修复的。现在，使用 `npm run lint-fix` 修复它，`a` 的去留就看自己手动更改。

你还可以运行 `npm run lint-html` 命令，将检查结果写入一个网页文件。

![npm run lint-html](https://upload-images.jianshu.io/upload_images/18281896-b46d8261f877a95b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 配置文件优先级

如果您按上面的步骤一步步来，你会可能已经知道，`ESLint` 支持几种格式的配置文件。

现在存在一个问题，如果同个目录下有多个 `ESLint` 文件，它们会如何执行，优先级如何？

ESLint [源码](https://github.com/eslint/eslint/blob/v6.0.1/lib/cli-engine/config-array-factory.js#L52)中给出了我们答案，其优先级配置如下：

```js
const configFilenames = [
  '.eslintrc.js',
  '.eslintrc.yaml',
  '.eslintrc.yml',
  '.eslintrc.json',
  '.eslintrc',
  'package.json'
]
```

> .eslintrc.js > .eslintrc.yaml > .eslintrc.yml > .eslintrc.json > .eslintrc > package.json

## 规则

ESLint 中的规则是单独添加的。默认情况下不强制执行任何规则。您必须明确指定规则，然后才会为 linting 过程启用它。

> [点击此处](http://eslint.org/docs/rules/)官方文档查找完整的规则列表。

在决定要包含哪些规则之后，您必须设置这些错误级别。每个错误级别可定义如下：

- `0` — 关闭规则，相当于 `off`
- `1` — 打开规则作为警告，相当于 `warn`
- `2` — 打开规则作为错误，相当于 `error`

错误和警告之间的区别在于 eslint 完成时将具有的退出代码。如果发现任何错误，eslint 将以 `1` 退出代码退出，否则将以 `0` 退出。

如果您在生成步骤中进行 lint，这允许您控制哪些规则应**破坏您的生成**，哪些规则应视为警告。

## 环境

您正在编写的代码可能适用于特定环境，例如，您可能正在使用 Express 框架在 Node.js 应用程序中编写 REST API，并且该应用程序的前端将在 Vue/React 中构建。

两个不同的项目、两个不同的环境，它们都可以在一个文件中具有单独的 eslint 配置，即使客户端和服务器位于一个被视为项目根目录的项目目录下。

**它是如何完成的？**

通过在 `.eslintrc` 的 `"env"` 部分将环境 `id` 设置为 `true`。

## ESLint CLI

ESLint 附带一个命令行界面（CLI），用于 lint 文件或目录。

```bash
eslint index.js
```

前面示例中我们已经看到，运行命令后生成的输出将按文件分组，并将指定 `line:column` 警告/错误、错误原因以及每个故障的规则名称。

## 将 ESLint 与您喜欢的编码风格结合使用

ESLint 个人并不提倡任何编码风格。您可以设置 `.eslintrc` 文件以使用您喜欢的[样式规则](http://eslint.org/docs/rules/#stylistic-issues)强制编码样式。

您还可以将 ESLint 与样式指南（如 [Airbnb](https://github.com/airbnb/javascript)、[JavaScript Standard Style](http://standardjs.com/)）一起使用。

你还必须使用额外的插件，例如：

- Airbnb 的插件 [`eslint-config-airbnb-base`](https://www.npmjs.com/package/eslint-config-airbnb-base)。
- JavaScript 标准风格 [eslint-config-standard](https://github.com/feross/eslint-config-standard)

有许多一些流行库的插件：[Vue](https://www.npmjs.com/package/eslint-plugin-vue) | [React](https://www.npmjs.com/package/eslint-plugin-react) | [Angular](https://github.com/EmmanuelDemey/eslint-plugin-angular)

您还可以在 [awesome-eslint](https://github.com/dustinspecker/awesome-eslint) 找到一些 ESLint 配置、插件等列表。

## 团队规范

[AlloyTeam](https://github.com/AlloyTeam) 给出的 React/Vue/TypeScript 项目的渐进式 ESLint 配置（[eslint-config-alloy](https://github.com/AlloyTeam/eslint-config-alloy)），以下贴出 React 的一小部分配置：

```js
module.exports = {
  parserOptions: {
    babelOptions: {
      presets: ['@babel/preset-react']
    }
  },
  plugins: ['react'],
  rules: {
    /**
     * 布尔值类型的 propTypes 的 name 必须为 is 或 has 开头
     * @reason 类型相关的约束交给 TypeScript
     */
    'react/boolean-prop-naming': 'off',
    /**
     * <button> 必须有 type 属性
     */
    'react/button-has-type': 'off',
    /**
     * 一个 defaultProps 必须有对应的 propTypes
     * @reason 类型相关的约束交给 TypeScript
     */
    'react/default-props-match-prop-types': 'off',
    /**
     * props, state, context 必须用解构赋值
     */
    'react/destructuring-assignment': 'off',
    /**
     * 组件必须有 displayName 属性
     * @reason 不强制要求写 displayName
     */
    'react/display-name': 'off'
    // ...
  }
}
```
