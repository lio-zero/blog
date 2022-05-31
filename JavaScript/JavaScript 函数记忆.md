# JavaScript 函数记忆

[Memoization](https://zh.wikipedia.org/wiki/%E8%AE%B0%E5%BF%86%E5%8C%96) — 函数记忆或函数缓存，是一种优化技术，用于许多编程语言中，以减少冗余、昂贵的函数调用。这是通过缓存函数的返回值来实现的。

以下是一个昂贵的函数调用，它以一种非常低效的方式找到数字的平方：

```js
const inefficientSquare = (num) => {
  let total = 0
  for (let i = 0; i < num; i++) {
    for (let j = 0; j < num; j++) {
      total++
    }
  }
  return total
}
```

我们可以使用相同的值运行此函数，每次都需要一段时间才能执行。

```js
const start = new Date()
inefficientSquare(40000)
console.log(new Date() - start) // 1278

const start2 = new Date()
inefficientSquare(40000)
console.log(new Date() - start2) // 1245
```

每一次执行，它都会重新计算！

## 为 memoize 编写伪代码

在编写任何代码之前，让我们先通过 Memoizer 进行推理。

- 引用函数作为输入
- 返回一个函数
- 创建某种形式的缓存以保存先前函数调用的结果
- 以后调用函数时，如果存在缓存结果，则返回缓存结果
- 如果缓存的值不存在，则调用函数并将结果存储在缓存中

## 编码时间

这是上述伪代码概述的实现。如引言中所述，这是次优的，您**不应该在生产中使用它**。我会解释为什么！

```js
const memoize = (func) => {
  // 创建结果缓存
  const results = {}
  // 返回函数
  return (...args) => {
    // 为结果缓存创建键
    const argsKey = JSON.stringify(args)
    // 仅在没有缓存值时执行 func
    if (!results[argsKey]) {
      // 将函数调用结果存储在缓存中
      results[argsKey] = func(...args)
    }
    // 返回缓存值
    return results[argsKey]
  }
}
```

这个实现会有一些缺点，它使用 `JSON.stringify` 在结果缓存中创建键。而 `JSON.stringify` 最大问题是它不能序列化某些输入，如函数和 `Symbol`（以及 JSON 中找不到的任何内容）。

## 测试效果

```js
const inefficientSquare = memoize((num) => {
  let total = 0
  for (let i = 0; i < num; i++) {
    for (let j = 0; j < num; j++) {
      total++
    }
  }
  return total
})

const start = new Date()
inefficientSquare(40000)
console.log(new Date() - start) // 1251

const start2 = new Date()
inefficientSquare(40000)
console.log(new Date() - start2) // 0
```

我们可以看到，在执行第二次时，输入相同的内容，它不需要时间重新计算；我们只是从一个对象中提取缓存的值。

> **注意**：它可以节省计算时间，提高读取性能，读取速度更快。但它同时也会消耗更多的内存来保存以前的结果（用内存换区性能）。

## 应用场景

它常用于解决同参数的多次执行问题，大量的循环或递归调用的数学运算或判断。

以下是一个斐波那契数列示例：

```js
const factorial = memoize((n) => (n <= 1 ? 1 : n * factorial(n - 1)))

factorial(5)
```

## 函数记忆仅在纯函数中有效

函数记忆很棒，但仅在您的纯函数中有效。换句话说，如果函数的返回值依赖于多个输入，那么这些输入的缓存值并不总是正确的。此外，如果您的函数有副作用，那么 memoizer 不会复制这些副作用，它只会返回最终返回的函数值。

## 更多资料

- [JavaScript 专题之函数记忆](https://github.com/mqyqingfeng/Blog/issues/46)
- [JavaScript 纯函数](https://github.com/lio-zero/blog/blob/master/JavaScript/JavaScript%20%E7%BA%AF%E5%87%BD%E6%95%B0.md)
