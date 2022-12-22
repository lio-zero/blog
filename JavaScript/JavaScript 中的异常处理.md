# JavaScript 中的异常处理

**错误是编程过程的一部分**。编写程序的过程难免会出现一些错误，通过这些产生的错误，我们可以学会如何避免遇到这样的情况，以及如何在下次做的更好。

在 JavaScript 中，当代码语句紧密耦合并产生错误时，继续使用剩余的代码语句是没有意义的。相反，我们试图尽可能优雅地从错误中恢复过来。JavaScript 解释器在出现此类错误时检查[异常处理](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Control_flow_and_error_handling)代码，如果没有异常处理程序，程序将返回导致错误的任何函数。

对[调用堆栈](https://developer.mozilla.org/en-US/docs/Glossary/Call_stack)上的每个函数重复此操作，直到找到异常处理程序或到达顶层函数，从而导致程序以错误终止，导致程序的崩溃。

一般来说，有两种处理方式：

- **抛出异常** — 如果在运行时发生的问题无法得到有意义的处理，最好抛出它。

```js
function openFile(fileName) {
  if (!exists(fileName)) {
    throw new Error('找不到文件 ' + fileName)
  }

  // do something
}
```

- **捕获异常** — 抛出的异常在运行时更有意义的地方被捕获和处理。

```js
try {
  openFile('../test.js')
} catch (e) {
  // 捕获异常
}
```

让我们更详细地了解这些操作。

## 抛出异常

你可能会看到类似 `ReferenceError: specs is not defined` 这样的情况。这表示通过 `throw` 语句引发的异常。

### 语法

`throw` 语法如下：

```js
throw «value»
```

`throw` 对可以作为异常抛出的数据类型没有限制：

```js
if (somethingBadHappened) {
  throw 'Something bad happened'
}
```

但 JavaScript 具有特殊的内置异常类型，其中之一是 `Error`，正如你在前面的示例中所看到的。这些内置的异常类型为我们提供了比异常消息更多的细节。

### Error

`Error` 类型用于表示一般异常。这种类型的异常最常用于实现用户定义的异常。它有两个内置属性可供使用。

- `message` — 作为参数传递给 `Error` 构造函数的内容。例如，`new Error('这是一条错误消息')`。你可以通过 `message` 属性访问消息。

```js
const myError = new Error('Error!!!')

console.log(myError.message) // Error!!!
```

- `stack` — 该属性返回导致错误的文件的历史记录（调用堆栈）。堆栈顶部还包括 `message`，后面是实际堆栈，从最新/隔离的错误点开始，到最外部负责的文件。

```js
Error: Error!!!
    at <anonymous>:1:1
```

**注意**，`new Error('...')` 在抛出之前不会执行任何操作，即 `throw new Error('error msg')` 将在 JavaScript 中创建一个 `Error` 实例，并停止脚本的执行，除非你对 `Error` 错误执行某些操作，例如捕获它。

另外，决不要尝试为基本 `Error` 类型实现**捕获所有**处理程序。这将混淆所发生的一切，并损害代码的可维护性和可扩展性。

## 捕获异常

现在我们知道了什么是异常以及如何抛出它们，让我们讨论一下如何通过捕获它们来阻止它们破坏我们的程序。

[`try-catch-finally`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/try...catch) 是处理异常的最简单方法。

```js
try {
  // 要运行的代码
} catch (e) {
  // 发生异常时要运行的代码，例如向监控系统发送一条 HTTP 请求上报异常
  reportErrorToMonitoringSystem(e)
}

[ // 可选
  finally {
    // 无论发生异常都始终执行的代码
  }
]
```

在 `try` 子句中，我们添加了可能产生异常的代码。如果发生异常，则执行 `catch` 子句。

有时，无论代码是否产生异常，都需要执行代码，这时我们可以使用可选块 `finally`。

即使 `try` 或 `catch` 子句执行 `return` 语句，`finally` 块也将执行。例如，以下函数返回 `'Execute finally'`，因为 `finally` 子句是最后执行的内容。

```js
function foo() {
  try {
    return true
  } finally {
    console.log('Execute finally')
  }
}
```

我们通常会在无法事先检查代码正确性的地方使用 `try-catch`。

```js
const user = '{"name": "D.O", "age": 18}'

try {
  // 代码运行
  JSON.parse(params) // 在出现错误的情况下，其余的代码将永远无法运行
  console.log(params)
} catch (err) {
  // 在异常情况下运行的代码
  console.log(err.message) // params is not defined
}
```

如上所示，在执行代码之前，不可能检查 `JSON.parse` 以获得 `stringify` 对象或字符串。

> **注意**：你可以捕获程序运行时产生的异常，但无法捕获 JavaScript 语法错误。

重要的一点：`try-catch-finally` 只能捕获同步错误。如果我们尝试将其用于异步代码，那么在异步代码完成其执行之前，`try-catch-finally` 可能已经执行了。

## 如何处理异步代码块中的异常

### 回调函数

使用回调函数（不推荐），我们通常会收到两个如下所示的参数：

```js
// 以 Node.js 的 readdir 为例，回调在 Node 中很常见。
fs.readdir(path, (error, files) => {
  if (error) throw error

  console.log(files)
})
```

我们可以看到有两个参数：`error` 和 `files`。如果有错误，`error` 参数将等于该错误，我们可以抛出该错误来进行异常处理。

在 `if (error)` 块中返回某些内容或将其他指令包装在 `else` 块中都很重要。否则，你可能会遇到另一个错误。例如，当你尝试访问 `files` 时，`files` 可能未定义。

### Promise

使用 promise 的 `then` 或者 `catch`，我们可以通过将错误处理程序传递给 `then` 方法或使用 `catch` 子句来处理错误。

```js
promise.then(onFulfilled, onRejected)
```

也可以使用 `.catch(onRejected)` 而不是 `.then(null, onRejected)` 添加错误处理程序，其工作方式相同。

让我们看一个 `.catch` 拒绝 `Promise` 的例子。

```js
Promise.resolve('1')
  .then((res) => {
    console.log(res) // 1
    throw new Error('go wrong') // 抛出异常
  })
  .then((res) => {
    console.log(res) // 不会被执行
  })
  .catch((err) => {
    console.error(err) // 捕获并处理异常 => Error: go wrong
  })
```

### 使用 `async/await` 和 `try-catch`

使用 `async/await` 和 `try-catch-finally` 处理异常是轻而易举的事。

```js
async function func() {
  try {
    await nonExistentFunction()
  } catch (err) {
    console.error(err) // ReferenceError: nonExistentFunction is not defined
  }
}
```

## 如何处理全局未捕获的异常

现在我们已经很好地理解了如何在同步和异步代码块中执行异常处理，让我们回答本文最后一个待解决的问题：**我们如何处理全局未捕获的异常？**

### 在浏览器中

我们可以使用 `window.onerror` 事件来处理未捕获的异常。每当运行时发生错误时，会在 `window` 对象上触发 `error` 事件。

```js
window.onerror = function () {
  // do something
}
// or
window.addEventListener('error', () => {
  // do something
})
```

`onerror` 的另一个常用做法是，当站点中的图片或视频等数据加载出错时，可以用该方法触发某些操作。例如，提供一张加载出错时的图片，或显示一条消息。

```html
<img src="logo.png" onerror="alert('加载图片时出错')" />
```

### 在 Node.js 中

`EventEmitter` 模块派生的 `process` 对象可以订阅事件 `uncaughtException`。

```js
process.on('uncaughtException', () => {})`
```

我们可以传递一个回调来处理异常。如果我们尝试捕获这个未捕获的异常，进程将不会终止，因此我们必须手动完成。

`uncaughtException` 仅适用于同步代码。对于异步代码，还有另一个称为 `unhandledRejection` 的事件。

```js
// 在 Node.js 中
process.on('unhandledRejection', (event, promise) => {
  // 第二个参数指向抛出错误的那个 promise
  console.log(event, promise)
  // do something
})

// 在浏览器中
window.addEventListener('unhandledrejection', (event) => {
  console.log(event, p)
  // do something
})
```

> 注意事件名大小写。

## 关键点

- `throw` 语句用于生成用户定义的异常。在运行时，当 `throw` 遇到语句时，当前函数的执行将停止，控制权将传递给 `catch` 调用堆栈中的第一个子句。如果没有 `catch` 子句，程序将终止。
- JavaScript 有一些内置的异常类型，最值得注意的是 `Error`，它返回 `Error` 中的两个重要属性：`stack` 和 `message`。
- `try` 子句将包含可能产生异常的代码，`catch` 子句会在发生异常时执行。`finally` 子句无论之前情况如何，最后都会执行。
- 对于 JS 运行时的同步异常，通过 `try-catch` 进行捕获，不过常规下它只能捕获同步代码。对于异步代码，使用 `async/await` 配合 `try-catch` 语句捕获异常。（Promise `then` 方法的第二个参数，或者 `catch` 捕获也可行）
- `try-catch` 无法捕获语法异常。
- `onerror` 可以捕获语法错误，也可以捕获运行时错误。而 `unhandledrejection` 可以监听到 Promise 中抛出的，未被 `.catch` 捕获的错误。
- 前端知名框架 Vue 和 React 都有对应的处理异常方案，比如 React 的 [Error Boundaries](https://reactjs.org/docs/error-boundaries.html)，Vue 的 [errorHandler](https://vuejs.org/api/application.html#app-config-errorhandler)、[warnHandler](https://vuejs.org/api/application.html#app-config-warnhandler)、[errorCaptured](https://vuejs.org/api/options-lifecycle.html#errorcaptured) 等，因此我们无需手动编写 `try/catch` 代码。另外，如果你对于 Vue 的组件错误监听感兴趣，可以阅读 [Vue 错误处理 — onErrorCaptured 钩子](https://github.com/lio-zero/blog/blob/main/Vue/Vue%20%E9%94%99%E8%AF%AF%E5%A4%84%E7%90%86%20%E2%80%94%20onErrorCaptured%20%E9%92%A9%E5%AD%90.md)。

捕获未处理的异常可以防止应用程序崩溃。

不要觉得麻烦，异常处理可以提高项目的稳定性，并且帮助你提高代码的可维护性、可扩展性和可读性。

## 更多资料

- [使用 promise 进行错误处理](https://zh.javascript.info/promise-error-handling)
- [错误处理，"try...catch"](https://zh.javascript.info/try-catch)
- [自定义 Error，扩展 Error](https://zh.javascript.info/custom-errors)
- [精读《捕获所有异步 error》](https://github.com/ascoders/weekly/blob/master/%E5%89%8D%E6%B2%BF%E6%8A%80%E6%9C%AF/209.%E7%B2%BE%E8%AF%BB%E3%80%8A%E6%8D%95%E8%8E%B7%E6%89%80%E6%9C%89%E5%BC%82%E6%AD%A5%20error%E3%80%8B.md)
- [精读《JavaScript 错误堆栈处理》](https://github.com/ascoders/weekly/blob/master/%E5%89%8D%E6%B2%BF%E6%8A%80%E6%9C%AF/6.%E7%B2%BE%E8%AF%BB%E3%80%8AJavaScript%20%E9%94%99%E8%AF%AF%E5%A0%86%E6%A0%88%E5%A4%84%E7%90%86%E3%80%8B.md)
