# Node.js assert 模块

[`assert` 模块](https://nodejs.org/api/assert.html)提供了一组用于验证不变量的断言函数。

该模块用于测试 Node.js 应用程序中功能的表达式。如果测试结果返回 `false`，`assert` 将抛出错误并停止程序。您可以将 `assert` 模块与 [Mocha](https://mochajs.org/) 和 [Chai](https://github.com/chaijs/chai) 等单元测试工具结合使用。

## 什么是不变量？

不变量是需要在程序中的某个点返回 `true` 的表达式或条件。让我们仔细看看下面的表达式：

```js
let a = 5
let b = 3
let correct = a > b
console.log(correct)
```

在终端或 Bash 上运行时，应该返回 `true`。如果由于某种原因，上面的应用程序返回 `false`，并且有其他一些表达式依赖于它，该怎么办？

如果我们将大于号 `>` 更改为 `<`，并运行此代码，我们将得到 `false`。让我们想象一下，上面符号的改变是一个错误，程序会因为这个错误返回一个意外的结果。开发团队如何追踪这个错误并确保不会再次发生？

上面的表达式是一个不变量的例子。为了保证代码按预期返回 `true`，Node.js 开发团队使用 `assert` 模块测试表达式。

## 严格断言模式和传统断言模式

严格模式下的断言涉及使用严格相等比较方法来验证不变量。在严格断言模式下，非严格方法的行为与其相对严格的方法相同。

例如，`assert.deepEqual()` 的行为类似于 `assert.deepStrictEqual()`，而 `assert.notEqual()` 的行为类似于 `assert.notStrictEqual()`。

两种模式如下：

```js
// 严格断言模式
const assert = require('assert').strict
const assert = require('assert/strict')

// 旧版断言模式
const assert = require('assert')
```

在旧模式中，不变量的比较涉及抽象相等比较的使用。建议始终使用严格模式来比较不变量的实际值和预期值。

以下是它们的两种模式的方法。

## 断言方法

[严格断言模式](https://nodejs.org/api/assert.html#strict-assertion-mode)：

- `assert.deepStrictEqual (actual, expected[, message])` 方法
- `assert.fail([message])` 方法
- `assert.ifError(value)` 方法
- `assert.doesNotMatch(string, regexp[, message])` 方法
- `assert.doesNotReject(asyncFn[, error][, message])` 方法

[传统断言模式](https://nodejs.org/api/assert.html#legacy-assertion-mode)：

- `assert.deepEqual(actual, expected[, message])` 方法
- `assert.equal(actual, expected[, message])` 方法
- `assert.notEqual(actual, expected[, message])` 方法
- `assert.notDeepEqual(actual, expected[, message])` 方法

详细示例文档都有，自己看去。
