# ESLint 配置文件

你可以使用 `.eslint.*` 文件或 `package.json` 文件中的 `eslintConfig` 选项来[配置 ESLint](https://eslint.org/docs/latest/user-guide/configuring)。你的 `.eslint.*` 文件可以是 `.eslintrc.json`、`.eslintrc.js` 或 `.eslintrc.yml`，也可以是 `.eslintrc`。

> 配置文件优先级：.eslintrc.js > .eslintrc.yaml > .eslintrc.yml > .eslintrc.json > .eslintrc > package.json。详细内容可以查看[如何在 Node.js 应用程序中使用 ESLint](https://github.com/lio-zero/blog/blob/main/Node/%E5%A6%82%E4%BD%95%E5%9C%A8%20Node.js%20%E5%BA%94%E7%94%A8%E7%A8%8B%E5%BA%8F%E4%B8%AD%E4%BD%BF%E7%94%A8%20ESLint.md)

下面是一个简单的 `.eslintrc.json` 文件，它启用了 [`no-unused-vars` ESLint 规则](https://eslint.org/docs/latest/rules/no-unused-vars):

```json
{
  "parserOptions": {
    "ecmaVersion": 2020
  },
  "rules": {
    "no-unused-vars": "error"
  }
}
```

你也可以将 ESLint 配置定义为一个导出文件的 JavaScript 对象。下面是等价的 `.eslintrc.js` 文件。

```js
module.exports = {
  parserOptions: {
    ecmaVersion: 2020
  },
  rules: {
    no-unused-vars: 'error'
  }
}
```

如果你更喜欢 [YAML](https://yaml.org/)，你也可以写一个 `.eslintrc.yml` 文件。

```yaml
parserOptions:
  ecmaVersion: 2020
rules:
  no-unused-vars: error
```

给定上面的每一个 ESLint 配置文件，在下面的脚本 `test.js` 中运行 ESLint 将打印一个 **'message' is assigned a value but never used** 错误。

```js
const message = 'Hello, World'
```

下面是 `eslint` 从上述 `test.js` 文件的命令行运行时的输出。

```bash
$ ./node_modules/.bin/eslint ./test.js

/scratch/test.js
  1:7  error  'message' is assigned a value but never used  no-unused-vars

✖ 1 problem (1 error, 0 warnings)

$ echo $?
1
```

> **Tips**：`echo $?` 命令是在 [Linux 中获取最后一个命令的退出代码的方式](https://shapeshed.com/unix-exit-codes/)。退出代码 `0` 意味着命令成功，即使出现警告，`eslint` 也成功了。如果退出代码为 `1`，则表示执行失败。

## 规则

[rules](https://eslint.org/docs/latest/rules/) 是 ESLint 的基本构建块。每个 ESLint 配置都是一组规则，以及这些规则执行的严格程度（错误或警告）。甚至 [JavaScript Standard Style](https://github.com/standard/standard/blob/master/RULES.md) 也是作为 ESLint 规则的集合。

规则配置可以是字符串（或用相应数字代替）或数组。如果规则配置为字符串，则必须为 `'off'`、`'warn'` 或 `'error'`。

- `"off"` 或 `0` — 告诉 ESLint 忽略给定的规则
- `"warn"` 或 `1` — 告诉 ESLint 将违反给定的行为视为警告（不影响退出代码）
- `"error"` 或 `2` — 告诉 ESLint 在违反给定规则时出错（触发时退出代码为 1）

例如，下面是一个 `.eslintrc.json`，它将 `no-unused-vars` 视为警告。

```json
{
  "parserOptions": {
    "ecmaVersion": 2020
  },
  "rules": {
    "no-unused-vars": "warn"
  }
}
```

如果规则配置是一个数组，数组的第一个元素是一个字符串或对应数字（`'off'`， `'warn'` 或 `'error'`），第二个元素是配置单个规则的选项。例如，下面的 `.eslintrc.json` 告诉 ESLint，当任何一行代码的长度超过 66 个字符时，使用 [`max-len` 规则](https://eslint.org/docs/latest/rules/max-len) 就会出错。

```json
{
  "parserOptions": {
    "ecmaVersion": 2020
  },
  "rules": {
    "max-len": ["error", { "code": 66 }]
  }
}
```

## 使用 `extends`

列出您想要使用的每一个 ESLint 规则通常是不可行、不推荐的，所以 ESLint 提供了一个 [`extends` 选项](https://eslint.org/docs/latest/user-guide/configuring#extending-configuration-files)，允许您扩展现有的 ESLint 配置，并进行覆盖。

出于实际目的，如果你正在构建自己的 ESLint 配置，我建议使用 ESLint 内置的 `eslint:recommended` 配置作为起点。

```json
{
  "extends": "eslint:recommended"
}
```

你可以在 [ESLint 推荐的配置中找到一个完整的规则列表](https://eslint.org/docs/latest/rules/)。你可以通过指定你自己的 `rules` 属性来覆盖 ESLint 推荐的配置中的单个规则。例如，下面的 ESLint 配置使用推荐的配置，除了禁用 `no-undef` 规则。

```json
{
  "extends": "eslint:recommended",
  "rules": {
    "no-undef": "off"
  }
}
```

## `parser` 选项

`parserOptions` 配置选项告诉 ESLint 你的目标是什么版本的 JavaScript。例如，当您设置 `parserOptions.ecmaVersion` 为 `2017` 时，下面的 JavaScript 是有效的：

```js
;(async function () {
  console.log('Hello, World!')
})()
```

但是，如果您更改 `parser.ecmaVersion` 为 `2016`，ESLint 将会失败并出现以下错误，因为异步函数在 ES2017 才被引入。

```bash
$ ./node_modules/.bin/eslint ./test.js

/scratch/test.js
  1:8  error  Parsing error: Unexpected token function

✖ 1 problem (1 error, 0 warnings)
```

ESLint 还内置了对 [JSX](https://thecodebarbarian.com/overview-of-jsx-with-non-react-examples.html) 的支持。例如，假设您有以下 `test.js` 文件：

```jsx
const hello = () => <h1>Hello, World</h1>
```

通常，ESLint 会在上面的脚本中抛出一个错误 `Parsing error: Unexpected token <`。但是您可以通过将 `parserOptions.ecmaFeatures.jsx` 设置为 `true` 来启用 JSX，如下所示。

```json
{
  "parserOptions": {
    "ecmaVersion": 2020,
    "ecmaFeatures": {
      "jsx": false
    }
  }
}
```

## 环境

仅仅指定 `ecmaVersion` 并不总是足够的。不同的 JavaScript 运行时和框架具有不同的全局变量和语义。例如，下面的脚本在 Node.js 中运行良好，但在浏览器中却不行，因为浏览器没有全局变量 `process`。

```js
process.env.MESSAGE = 'Hello, World'
```

使用下面的 ESLint 配置，您将收到 `'process' is not defined` 错误。

```json
{
  "parserOptions": {
    "ecmaVersion": 2020
  },
  "rules": {
    "no-undef": "error"
  }
}
```

但是一旦你告诉 ESLint 这个脚本将使用 `"env": {"node": true}` 在 node .js 中运行，ESLint 就不会在上面的脚本中出错。

```json
{
  "parserOptions": {
    "ecmaVersion": 2020
  },
  "env": {
    "node": true
  },
  "rules": {
    "no-undef": "error"
  }
}
```

另一个常用的 `env` 是 `browser`，它告诉 ESLint 这个脚本将在浏览器中运行。这使您的脚本可以访问仅限浏览器的全局变量，例如 `window`。

```json
{
  "parserOptions": {
    "ecmaVersion": 2020
  },
  "env": {
    "browser": true
  },
  "rules": {
    "no-undef": "error"
  }
}
```

ESLint 档有一个支持环境的[完整列表](https://eslint.org/docs/latest/user-guide/configuring#specifying-environments)。

## 插件

ESLint 自带了各种各样的[内置规则](https://eslint.org/docs/latest/rules/)，但你也可以找到许多在 npm 上有额外规则的插件。许多 ESLint 插件为使用特定的库和框架提供了额外的规则。

例如，[eslint-plugin-vue](https://www.npmjs.com/package/eslint-plugin-vue) 提供了额外的特定于 Vue 规则。运行 `npm i eslint-plugin-vue -D` 后，你可以在 ESLint 配置中添加一个 `plugins` 列表，其中包括 `'eslint-plugin-vue'`，或者简称为 `'vue'`，因为 ESLint 足够聪明，可以为你添加 `'eslint-plugin-'` 前缀。

```json
{
  "parserOptions": {
    "ecmaVersion": 2020
  },
  "plugins": ["vue"]
}
```

一旦你这样做了，你就可以访问特定于 Vue 的规则，比如 [`no-async-in-computed-properties`](https://eslint.vuejs.org/rules/no-async-in-computed-properties.html)。下面的 ESLint 配置打开了 `no-async-in-computed-properties` 规则。

```json
{
  "parserOptions": {
    "ecmaVersion": 2020
  },
  "plugins": ["eslint-plugin-vue"],
  "rules": {
    "vue/no-async-in-computed-properties": "error"
  }
}
```

如果你在下面的 `test.js` 文件中运行 ESLint，`vue/no-async-in-compute -properties` 规则将会出错，因为 `badProperty` 被设置为一个异步函数:

```js
const Vue = require('vue')

module.exports = Vue.component('bad-component', {
  template: '<h1>Hello</h1>',
  computed: {
    badProperty: async function () {
      return 42
    }
  }
})
```

```bash
$ ./node_modules/.bin/eslint ./test.js

/scratch/test.js
  6:18  error  Unexpected async function declaration in "badProperty" computed property  vue/no-async-in-computed-properties

✖ 1 problem (1 error, 0 warnings)
```
