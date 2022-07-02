# JavaScript 中的函数和变量提升

[提升](https://developer.mozilla.org/zh-CN/docs/Glossary/Hoisting)是一个 JavaScript 概念，其中变量、函数和类定义在执行之前被提升到它们的词法环境（它们的作用域）的顶部。

在 JavaScript 中有多种定义函数的方法。我们将看看以下三种方法：

- 函数声明
- 函数表达式
- 箭头函数

```js
// 函数声明
function Motto() {
  console.log('All work and no play makes Jack a dull boy')
}

// 函数表达式
var Motto = function () {
  console.log('All work and no play makes Jack a dull boy')
}

// 箭头函数
var Motto = () => console.log('All work and no play makes Jack a dull boy')

Motto() // All work and no play makes Jack a dull boy
```

乍一看，上述定义函数的方式看起来是一样的。然而，两者之间存在着微妙的差异。

本文将着重关注函数声明和函数表达式。我们先来看一个例子：

```js
double(5) // 10
square(2) // Uncaught ReferenceError: Cannot access 'square' before initialization at <anonymous>:3:1

const square = function (x) {
  return x * x
}

function double(x) {
  return 2 * x
}
```

正如我们所看到的，这个程序并没有像预期的那样工作。

但是，如果我们注释掉对 `square` 函数的调用，或者将其移到定义之下，我们可以看到程序按预期工作。

出现这种异常的原因是，我们可以在函数声明被实际定义之前调用它，但是我们不能对函数表达式执行同样的操作。这与 JavaScript 解析器有关，它解析给定的脚本。

函数声明被提升，而函数表达式不被提升。JavaScript 引擎通过在实际执行脚本之前将函数声明提升到当前作用域来提升函数声明。

结果，上面的代码片段实际上解析如下：

```js
function double(x) {
  return 2 * x
}

double(5) // 10
square(2) // Uncaught ReferenceError: Cannot access 'square' before initialization at <anonymous>:3:1

const square = function (x) {
  return x * x
}
```

但是 `square` 函数没有被提升，这就是为什么它只能从定义向下到程序的其余部分。这导致调用时出错。

函数表达式就是这种情况。

JavaScript 中还有另一种形式的提升，当使用关键字 `var` 声明变量时会发生这种提升。

让我们看几个例子来说明这一点：

```js
var language = 'JavaScript'

function whichLanguage() {
  if (!language) {
    var language = 'Go'
  }
  console.log(language)
}
whichLanguage() // Go
```

当我们运行上述代码时，我们可以在控制台看到 `JavaScript` 被替换为 `Go`。

我们要知道，函数声明的方式与使用关键字 `var` 声明变量的方式相同。

关于提升方式的不同，有几点需要注意：

- 当函数声明被提升时，整个函数体被移动到当前作用域的顶部。
- 提升时使用关键字 `var` 声明的变量只会将变量名称移动到当前作用域的顶部，而不是赋值。
- 在函数内部，使用关键字 `var` 声明的变量的作用域仅限于函数，而不是 `if` 块或 `for` 循环。
- 函数提升取代了变量提升。

牢记这些规则，让我们看看 JavaScript 引擎将如何解析上述代码：

```js
var language = 'JavaScript'

function whichLanguage() {
  var language
  if (!language) {
    language = 'Go'
  }
  console.log(language)
}

whichLanguage()
```

可以看到，`var language` 被移动到了当前作用域的顶部，因此它的值为 `undefined`。这使得它进入 `if` 块，从而将其重新分配为 `Go` 值。

让我们看另一个进一步证明这一点的例子：

```js
var name = 'K.O'
function myName() {
  name = 'O.K'
  return
  function name() {}
}

myName()
console.log(name) // K.O
```

通过遵循 JavaScript 引擎如何解析文件的规则，我们可以推断上述代码将产生什么。

让我们看看它是如何解析的：

```js
var name = 'K.O'
function myName() {
  function name() {} // 提升 name 函数
  name = 'O.K' // 重新分配给新值的 name
  return
}

myName()
console.log(name) // K.O
```

`K.O` 将被注销，因为 `myName` 函数中定义的 `name` 受该函数的作用域限制，并在函数执行后被丢弃。

## 最后

以上内容涵盖了在 JavaScript 中使用提升时要考虑的大部分事项。这些规则有一些例外，但是随着 ES6 的引入，我们现在可以通过在声明变量时使用 `const` 和 `let` 关键字来避免许多这些警告。
