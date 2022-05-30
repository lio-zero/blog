# JavaScript 中的 setTimeout 和 setInterval 方法

有时您不希望函数立即运行。您希望它重新执行，甚至在特定时间间隔后重复运行。JavaScript 为我们提供了两种实现方法： `setTimeout` 和 `setInterval`。下面，我们将来理解这两个方法。

## setTimeout

`setTimeout` 方法在停止后运行给定的函数。它设置一个定时器，并在时间终止时执行给定的函数（回调）。

`setTimeout` 方法的语法如下：

```js
const timerID = setTimeout(fn, delay, arg1, arg2, ...)
```

- `fn` — 在给定的延迟时间后要执行的函数。
- `delay`（可选） — 定时器在执行函数之前应该等待的时间（以毫秒为单位）。如果未指定，则使用替换值 0。
- `arg1, arg2, ...argN`（可选） — 给定函数的参数。

`setTimeout` 方法返回一个整数值，用于标识创建的定时器。这个整数用于清除定时器。

**示例**

```js
function sayHello() {
  console.log('Hello')
}

setTimeout(sayHello, 2000) // 'Hello'
```

2 秒后，将在控制台打印 `'hello'`。

如果这个 `sayHello` 函数接受参数，我们可以创建：

```js
function sayHello(name) {
  console.log(`Hello, ${name}`)
}

setTimeout(sayHello, 1000, 'IU')
```

这将在 2 秒后打印 `'Hello，IU'`。

`setTimeout` 还接受第三个参数，作为 `fn` 函数的参数：

```js
for (var i = 0; i < 3; i++) {
  setTimeout(console.log, 1000 * i, i) // 0  1 2
}
```

它可以巧妙的解决一个闭包问题。通过给 `setTimeout` 的回调函数传参的方式，保存了每次循环中 `i` 的值。

## clearTimeout

`clearTimeout` 方法用于取消定时器。例如，我们可能希望在定时器执行函数之前取消它。`setTimeout` 方法会返回一个 `ID`，这个 `ID` 可以用于取消定时器。

```js
function sayHello(name) {
  console.log(`Hello, ${name}`)
}

const timerID = setTimeout(sayHello, 3000, 'IU')

clearTimeout(timerID)
```

以上示例中，定时器永远不会运行。

## setInterval

`setInterval` 方法用于安排函数在一段时间后**重复**执行。该方法的语法如下：

```js
const timerID = setInterval(fn, delay, arg1, arg2, ...)
```

在这种情况下，延迟是定时器应该延迟函数的连续执行的时间（毫秒）。`setInterval` 方法还返回用于清除定时器的 ID。

**示例**

```js
function sayHello(name) {
  console.log(`Hello, ${name}`)
}

setInterval(sayHello, 3000, 'IU') // 'IU'
```

上面的代码将在 3 秒钟后重复打印 `'Hello IU'`。

`setInterval` 的语法与 `setTimeout` 非常相似。主要区别在于：

- `setTimeout` 方法只在定时器过期后触发一次。
- `setInterval` 方法会重复运行函数，除非它被取消。

为了阻止 `setInterval` 持续运行，我们可以使用 `clearInterval` 方法。

## clearInterval

`clearInterval` 方法用于停止 `setInterval` 方法的执行。它接受定时器 `ID` 作为参数，并使用该 `ID` 停止定时器。

```js
let counter = 0
function sayHello(name) {
  console.log(`Hello, ${name}`)
  counter++

  if (counter === 3) {
    clearInterval(timerID)
  }
}

let timerID = setInterval(sayHello, 3000, 'IU')
console.log(1)
```

`sayHello` 函数只执行 3 次。

您可能会想知道 `if` 语句为什么在函数内部，而不是函数之外，例如：

```js
let counter = 0
function sayHello(name) {
  console.log(`Hello, ${name}`)
  counter++
}

let timerID = setInterval(sayHello, 3000, 'IU')

if (counter === 3) {
  clearInterval(timerID)
}
```

此时，我们需要了解 JavaScript 执行 `setInterval` 和 `setTimeout` 方法的方式。

## 非阻塞 I/O 操作

与其他语言不同，JavaScript 有一个执行任务的线程，它逐行执行任务。这意味着一行代码必须在继续下一行之前完成执行。换句话说，JavaScript 代码的执行是阻塞的。

但是，有一些非阻塞 I/O 操作是由底层引擎处理的。例如 `AJAX`、`setTimeout` 和 `setInterval` 获取数据等操作属于这一类。因此，JavaScript 不会等待传递给 `setTimeout` 或 `setInterval` 方法的函数（回调函数）完成执行，然后再转到下一个任务或代码行。

在上面的例子中，如果我们以第二种方式编写，定时器将不会停止运行。这是因为在执行这一行 `let timerID = setInterval(sayHello, 3000, 'IU')` 之后，JavaScript 立即转到下一个代码块.

```js
if (counter === 3) {
  clearInterval(timerID)
}
```

在这一点上，条件不是真的，所以定时器永远不会被清除。这也是为什么在我们的 `clearTimeout` 示例中，传递给 `setTimeout` 的回调函数从未被触发的原因。因为 JavaScript 立即转到下一行代码。
