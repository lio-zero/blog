# 如何在 JavaScript 中使用管道（管道运算符）？

管道运算符（`|>`）在函数式编程中很常见，但他目前还没内置在 JavaScript 中，[正处于 TC39 审议的草案/第 2 阶段](https://tc39.es/proposal-pipeline-operator/#sec-intro)。

虽然还没内置的 JavaScript 管道运算符，但我们可以使用现有的方法手动的实现它。你也可以使用 [Babel 插件](https://babeljs.io/docs/en/babel-plugin-proposal-pipeline-operator)。

## 什么是管道？

管道是将一个函数的输出直接发送到另一个函数。在伪代码中，可以表示为：

> output = input -> func1 -> func2 -> func3

在这种情况下，将 `input` 通过管道输送到 `func1`，将 `func1` 通过管道输送到 `func2` 的输出，在将 `func2` 通过管道输送到 `func3`。

在不支持管道的情况下，我们可以这么做：

```js
const output = func3(func2(func1(input)))
```

这是很难阅读且不直观的！如果我们能在 JavaScript 中使用实际的管道操作符（`|>`），那就太好了，但实际上我们还不能使用它。

## 手动实现

- 使用扩展运算符（`...`）允许将任意数量的参数传递到创建 `pipe` 函数中，传入的参数都存放在 `args` 数组中。
- 使用 [`Array.prototype.reduce()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce) 方法遍历数组 `args`。执行 `reduce` 时，累加器会将前一个累加器传递给当前元素的结果！

```js
const pipe = (...args) => args.reduce((acc, el) => el(acc))

const title = 'Front End Interview'

const toLowerCase = (str) => str.toLowerCase()
const addHyphens = (str) => str.replace(/\s/g, '-')

pipe(title, toLowerCase, addHyphens) // "front-end-interview"
```

如果使用管道运算符，它将如此的简单：

```txt
title |> toLowerCase |> addHyphens
```

有一个异步的实现方法，感兴趣的可以看看：[pipeAsyncFunctions](https://www.30secondsofcode.org/js/s/pipe-async-functions)

## 更多资料

- [Hack Pipe for Functional Programmers: How I learned to stop worrying and love the placeholder](https://jamesdigioia.com/hack-pipe-for-functional-programmers-how-i-learned-to-stop-worrying-and-love-the-placeholder/) — James DiGioia 介绍了相比于传统的 [Ramda](https://ramdajs.com/) 和 [Lodash/fp](https://github.com/lodash/lodash/wiki/FP-Guide) 函数式编程库引入的柯里化概念，JavaScript 引入管道运算符的好处。
- [I'VE USED THE PIPE() FUNCTION 2,560 TIMES AND I CAN TELL YOU IT'S GOOD!](https://www.obvibase.com/dev-blog/i-ve-used-the-pipe-function-2-560-times-and-i-can-tell-you-it-s-good)
