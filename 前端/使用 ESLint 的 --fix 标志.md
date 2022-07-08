# 使用 ESLint 的 --fix 标志

[ESLint 的`--fix`选项](https://eslint.org/docs/user-guide/command-line-interface#options)告诉 ESLint 修复代码中任何它知道如何修复的错误。

例如，ESLint 推荐的配置使用了 [`no-extra-boolean-cast` 规则](https://eslint.org/docs/rules/no-extra-boolean-cast)，这将在 `if` 语句中删除不必要 `!!`。例如，假设您有以下 `test.js` 文件。因为 JavaScript `if` 语句已经检查了 truthy 值，所以在 `if` 语句中没有必要使用 `!!`。

```js
if (!!(typeof window === 'undefined')) {
  console.log('Hello from Node.js!')
}
```

假设您有以下 `.eslintrc.json` 配置文件：

```json
{
  "parserOptions": {
    "ecmaVersion": 2020
  },
  "rules": {
    "no-extra-boolean-cast": "error"
  }
}
```

ESLint 会报 **Redundant double negation** 错误：

```bash
$ ./node_modules/.bin/eslint ./test.js

/scratch/test.js
  1:5  error  Redundant double negation  no-extra-boolean-cast

✖ 1 problem (1 error, 0 warnings)
  1 error and 0 warnings potentially fixable with the `--fix` option.

$ cat ./test.js
```

注意 **`1 error and 0 warnings potentially fixable with the --fix option`** 这一行。这说明 ESLint 知道如何修复这个错误。运行 `./node_modules/.bin/eslint --fix ./test.js`，这个错误就会消失。

```bash
$ ./node_modules/.bin/eslint --fix ./test.js
$ cat ./test.js

if (typeof window === 'undefined') {
  console.log('Hello from Node.js!');
}
```

请注意，ESLint 删除了不必要的 `!!`。

ESLint 只能自动修复某些 ESLint 规则的违规行为。ESLint 有一个完整的内置 [ESLint 规则列表](https://eslint.org/docs/rules/)，并解释了它可以自动应用修复的规则。

## 使用 npm 脚本

我们添加一个 `npm scripts` 来运行 ESLint 规则。

例如，假设您的 `package.json` 文件包含以下行：

```json
{
  "scripts": {
    "lint:fix": "eslint . --fix"
  }
}
```

现在，您只需要在命令行运行 `npm run lint:fix`，它将修复它可修复的内容。
