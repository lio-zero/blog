# JavaScript 中的 Iterator 和 Generator

## Iterator

迭代是编程中的一种常见做法，通常用于循环一组值，或者转换每个值，或者以某种方式使用或保存它以供以后使用。

在 JavaScript 中，我们经常使用类似以下的 `for` 循环：

```js
for (var i = 0; i < foo.length; i++) {
  // ...
}
```

但 ES6 为我们提供了另一种选择：

```js
for (const i of foo) {
  // ...
}
```

这无疑是一种更简洁、更易于使用的方式。但关于这种新的迭代，还有一点非常重要：它允许您直接与数据集的元素交互。

假设我们要找到数组中大于 `20` 的元素放置到另一个数组中：

```js
const arr = [73, 6, 90, 19, 15]
const newArr = []

for (var i = 0; i < arr.length; i++) {
  if (arr[i] > 20) {
    newArr.push(arr[i])
  }
}

newArr // [73, 90]
```

它可以工作，但它很笨重，而且这种笨重很大程度上取决于 JavaScript 处理 `for` 循环的方式。然而，对于 ES6，我们在新的迭代器中可以这样写：

```js
const arr = [73, 6, 90, 19, 15]
const newArr = []

for (const i of arr) {
  if (i > 20) {
    newArr.push(i)
  }
}

newArr // [73, 90]
```

这要干净得多，但最引人注目的是 `for` 循环。变量 `i` 现在表示数组中名为 `newArr` 的实际项。所以，我们不再需要通过索引来调用它。这意味着，我们可以直接调用 `i`，而不是调用循环中的 `newArr[i]`。

在这背后，这种迭代利用了 ES6 的 [Symbol.iterator()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Symbol/iterator) 方法。它负责描述迭代，并在被调用时返回一个 JavaScript 对象，其中包含循环中的 `next` 值和一个 `done` 键，该键是 `true` 还是 `false` 取决于循环是否完成。

### Symbol.iterator

以下是一个数组：

```js
const numbers = [1, 2, 3]
numbers[Symbol.iterator] // function values() { [native code] }
```

ES6 规范定义了对象的非字符串 `symbol` 属性名称，以描述特定行为。`Symbol.iterator` 是其中之一，它描述了迭代是如何工作的。

```js
const numbersIterator = numbers[Symbol.iterator]()

numbersIterator.next() // { value: 1, done: false }
numbersIterator.next() // { value: 2, done: false }
numbersIterator.next() // { value: 3, done: false }
numbersIterator.next() // { value: undefined, done: true }
```

以上是 `for (var num of numbers)` 在 JS 引擎下所做的事情。

当我们调用 `numbers[Symbol.iterator]` 时，会返回一个带有 `next` 方法的对象。调用 `next` 会给我们一个包含 `value` 的对象，或者表示没有其他值了。

### 自己实现一个

让我们创建一个对象来迭代字符串中的 `Words`（以一种非常简单的方式）。

首先，创建一个构造函数：

```js
function Words(str) {
  this._str = str
}
```

然后，创建一个迭代器工厂：

```js
Words.prototype[Symbol.iterator] = function () {
  const re = /\S+/g
  const str = this._str

  return {
    next() {
      const match = re.exec(str)
      if (match) {
        return { value: match[0], done: false }
      }
      return { value: undefined, done: true }
    }
  }
}
```

我们返回一个带有 `next` 方法的对象，它返回 `{ value: nextWordInTheString, done: false }`，直到没有 `next` 的方法。

```js
const helloWorld = new Words('Hello world')

for (var word of helloWorld) {
  console.log(word)
}
// 'Hello'
// 'world'
```

它可以正常工作！

## Generator

Generator 译为生成器，也称为“迭代器工厂”，是 ES6 JavaScript 函数的一个补充，用于创建特定的迭代。它们为你提供了特殊的、自定义的循环方法。

它是一种特殊的函数，能够暂停自身，稍后再恢复，同时允许其他代码运行。

代码决定它必须等待，因此它允许“队列中”的其他代码运行，并保留在“等待的事情”完成时恢复其操作的权利。

所有这些都是用一个简单的关键字完成的：`yield`。当生成器包含该关键字时，执行将暂停。

生成器可以包含许多 `yield` 关键字，因此会多次停止，它由 `function *` 关键字标识。

生成器支持 JavaScript 编程的全新范例，允许：

- 生成器运行时的双向通信
- 不会冻结程序的长寿命 `while` 循环

使用 `function*` 定义的生成器是创建迭代器工厂的一种更方便的方式。

从生成器中，你 `yield` 你想要提供的值，它会为你处理 `value/done` 对象:

```js
function* someNumbers() {
  yield 1
  yield 2
  yield 3
}

// 初始化
const iter = someNumbers()

iter.next() // { value: 1, done: false }
iter.next() // { value: 2, done: false }
iter.next() // { value: 3, done: false }
iter.next() // { value: undefined, done: true }
```

这一切都发生在您使用 `yield` 关键字暂停代码的运行，以及是否调用 `next` 方法再次启动迭代器。

我们可以使用 `for...of` 循环它：

```js
for (var num of someNumbers()) {
  console.log(num)
}
// 1
// 2
// 3
```

尽管上面的 `for...of` 循环看起来很直观，但它只是通过一点小技巧才能工作。`for...of` 调用对象的 `Symbol.iterator` 方法来获得迭代器，但我们给出的 `someNumbers()` 已经是一个迭代器了。为了解决这个问题，生成器返回的迭代器有一个 `Symbol.iterator` 方法返回自身，表示 `iter[Symbol.iterator]() === iter`。这有点奇怪，但它使上述代码按预期工作。

```js
iter[Symbol.iterator]() // Object [Generator] {}
iter[Symbol.iterator]() === iter // true
```

生成器通常是描述迭代的一种更简单的方法。对于我们的上述的 `Words` 示例，它要简单得多：

```js
Words.prototype[Symbol.iterator] = function* () {
  const re = /\S+/g
  const str = this._str
  let match
  while ((match = re.exec(str))) {
    yield match[0]
  }
}
```

> **注意**：箭头函数不能作为 Generator 函数，不能使用 `yield` 关键字。

另外，当值以异步方式出现时，将使用 `Symbol.asyncIterator` 替代 `Symbol.iterator`。

本文更多详细的内容可以阅读下文提供的资料。

## 更多资料

- [Generator 函数的语法](https://es6.ruanyifeng.com/#docs/generator)
- [Generator 函数的异步应用](https://es6.ruanyifeng.com/#docs/generator-async)
- [Generator，高级 iteration](https://zh.javascript.info/generators-iterators)
- [ES6 系列之 Generator 的自动执行](https://github.com/mqyqingfeng/Blog/issues/99)
- [Generators in JavaScript](https://yagrawal.dev/generators-in-javascript)
- [JavaScript 中循环之间的差异](https://github.com/lio-zero/blog/blob/main/JavaScript/JavaScript%20%E4%B8%AD%E5%BE%AA%E7%8E%AF%E4%B9%8B%E9%97%B4%E7%9A%84%E5%B7%AE%E5%BC%82.md)
- [迭代协议](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Iteration_protocols)
