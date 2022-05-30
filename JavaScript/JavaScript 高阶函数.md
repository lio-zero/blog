# JavaScript 高阶函数

**高阶函数**（Higher-order function）是将函数作为参数或返回函数的函数。

之所以可以使用 JavaScript 编写高阶函数，是因为函数是值，这意味着它们可以分配给变量并作为值传递。当引用作为参数传递的函数时，您可能还会经常听到术语**回调**，因为它是由高阶函数调用的。这在 JavaScript 中尤其常见，事件处理，异步代码和数组操作在很大程度上依赖于回调的概念。

**其主要优点**是它使我们能够创建合成以及更可重用和可读的代码的抽象层，可以轻松组合以创建更复杂的逻辑并且具有很高的可重用性

## 示例

我们还是使用在 [JavaScript 函数合成](https://www.jianshu.com/p/5409606673b5) 和 [如何在 JavaScript 中使用管道（管道运算符）？](https://www.jianshu.com/p/d48a8bcba6da) 中的例子。


```javascript
const compose = f => g => x => f(g(x))

const title = 'Front End Interview'

const toLowerCase = (str) => str.toLowerCase()
const addHyphens = (str) => str.replace(/\s/g, '-')

compose(toLowerCase)(addHyphens)(title) // "front-end-interview"
```

就是这样。两点：函数作为参数和返回函数

## JS 中内置的高阶函数

JavaScript 中很多内置的高阶函数，如 `reduce`、`filter`、`sort`、`map` 等。下面看看几个简单的示例：

```js
const add = (a, b) => a + b
const isEven = num => num % 2 === 0

const data = [2, 3, 1, 5, 4, 6]

data.filter(Boolean(isEven)) // [2, 4, 6]
data.filter(isEven).reduce(add) // 12
data.sort((a, b) => a - b) // [1, 2, 3, 4, 5, 6]
```

## 提升

你可以在 [30 seconds of code](https://www.30secondsofcode.org/js/p/1) 中的 JavaScript 部分找到很多关于高阶函数的有用示例。


- 高阶函数是将一个或多个函数作为参数的函数，它用于数据处理，也可能将函数作为返回结果。
- 高阶函数旨在抽象一些重复执行的操作
- 一个典型的例子是 `map`，它将一个数组和一个函数作为参数。`map` 使用这个函数来转换数组中的每个元素，并返回一个包含转换后元素的新数组。JavaScript 中的其他常见示例是 `forEach`、`filter` 和      `reduce`。

```javascript
let arr = [1, 2, 3, 4, 5]

function pow(x) {
    return x * x;
}

let results = arr.map(pow)
```

- 高阶函数不仅需要操作数组的时候会用到，还有许多函数返回新函数的用例。`Function.prototype.bind` 就是一个例子。
