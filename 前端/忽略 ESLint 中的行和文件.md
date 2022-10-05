# 忽略 ESLint 中的行和文件

[ESLint](https://eslint.org/) 根据预定义的规则分析代码以发现问题。然而，有时你需要打破 ESLint 规则。ESLint 支持几种机制来[忽略代码中的规则冲突](https://eslint.org/docs/latest/user-guide/configuring/ignoring-code#ignorepatterns-in-config-files)：

- 使用注释，可以禁用行或代码块的某些规则。
- 使用 [`.eslintignore` 文件](https://eslint.org/docs/latest/user-guide/configuring/ignoring-code#the-eslintignore-file)。
- 另外，如果您想忽略所有文件的某些语法规则，可以使用 [ESLint 配置文件](https://github.com/lio-zero/blog/blob/main/%E5%89%8D%E7%AB%AF/ESLint%20%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6.md)禁用规则。

## 使用注释禁用 ESLint

ESLint 允许您使用 `/* eslint */` 注释禁用单个 lint 规则。例如，许多 ESLint 规则[不允许使用 JavaScript 的 `eval()` 函数](https://github.com/lio-zero/blog/blob/main/JavaScript/JavaScript%20Eval.md)，因为 [`eval()` 有几个安全问题](https://alligator.io/js/eval/)。但是，如果您确实确定要允许 `eval()`，可以按如下方式禁用 lint 规则：

```js
const res = eval('42') // eslint-disable-line no-eval
```

`// eslint-disable-line` 注释仅对该行禁用 `no-eval` 规则。

您还可以使用 [`/* eslint-disable */`](https://eslint.org/docs/2.13.1/user-guide/configuring#disabling-rules-with-inline-comments)，禁用整个功能块的 `no-eval` 规则。

```js
function usesEval() {
  /* eslint-disable no-eval */
  const res = eval('42')
  const res2 = eval('test')

  return res2 + res
}
```

如果将 `/* eslint-disable no-eval */` 放在 `.js` 文件中的任何代码之前，这将禁用整个文件的 `no-eval` 规则。

您还可以通过将 `/* eslint-disable */` 置于文件顶部来禁用所有 ESLint 规则。

## 使用 `.eslintignore`

您可以使用注释来禁用文件的所有 ESLint 规则，但[通常不鼓励这样做](https://github.com/sindresorhus/eslint-plugin-unicorn/blob/master/docs/rules/no-abusive-eslint-disable.md)。如果您确定要让 ESLint 忽略一个文件，通常最好将其列在项目根目录中的 `.eslintignore` 文件中。

`.eslintignore` 语法类似于 [`.gitignore`](https://git-scm.com/docs/gitignore)。要忽略文件 `myfile.js`，您只需将以下行添加到 `.eslintignore`：

```txt
myfile.js
```

ESLint 支持通配文件。要忽略所有以 `.test.js` 结尾的文件，您可以将这一行行添加到 `.eslintignore` 中：

```txt
*.test.js
```

ESLint 认为 `.eslintignore` 中的路径相对于 `.eslintignore` 文件的位置。以下是忽略项目 `data` 目录中所有文件的方法。

```txt
data/*
```

您也可以在 `package.json` 中通过 `eslintIgnore` 字段配置需要忽略的文件。

```json
{
  "eslintIgnore": ["hello.js", "world.js"]
}
```

这通常在 ESLint 找不到根目录的 `.eslintIgnore` 文件且未指定替代文件时，才会查找 `package.json` 的 `eslintIgnore` 字段检查要忽略的文件。

## ESLint 配置文件

## 配置文件中的 `ignorePatterns`

您可以告诉 ESLint 在配置文件中使用 `ignorePatterns` 忽略特定的文件和目录。

```json
{
  "ignorePatterns": ["temp.js", "**/vendor/*.js"],
  "rules": {
    // ...
  }
}
```

`ignorePatterns` 模式遵循与 `.eslintignore` 相同的规则，但优先级低于 `.eslintignore`。

### 全局禁用规则

可以在 `.eslintrc.js` 文件单独配置全局禁用规则。

```json
{
  "rules": {
    "no-eval": 0
  }
}
```

您也可以在 `package.json` 的 `eslintConfig` 字段配置禁用规则，如下所示：

```json
{
  "eslintConfig": {
    "rules": {
      "no-eval": 0
    }
  }
}
```

完整的内容请查阅 [Ignoring Code](https://eslint.org/docs/latest/user-guide/configuring/ignoring-code)。
