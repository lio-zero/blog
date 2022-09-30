# JavaScript 组合函数

**组合（compose）函数**指的是将多个函数合成为一个函数。

在 JS 函数式编程中，你可以经常看到如下表达式运算。

```js
a(b(c(x)))
```

这看起来很不雅观，为了解决函数多层调用的嵌套问题，我们需要用到组合函数。其语法格式如下：

```js
const f = compose(a, b, c) // 组合函数 f(x)
```

我们使用昨天写的[如何在 JavaScript 中使用管道（管道运算符）？](https://github.com/lio-zero/blog/blob/master/JavaScript/%E5%A6%82%E4%BD%95%E5%9C%A8%20JavaScript%20%E4%B8%AD%E4%BD%BF%E7%94%A8%E7%AE%A1%E9%81%93%EF%BC%88%E7%AE%A1%E9%81%93%E8%BF%90%E7%AE%97%E7%AC%A6%EF%BC%89%EF%BC%9F.md) 中的示例：

```js
const compose = (f) => (g) => (x) => f(g(x))

const toLowerCase = (str) => str.toLowerCase()
const addHyphens = (str) => str.replace(/\s/g, '-')

const title = 'Front End Interview'
compose(toLowerCase)(addHyphens)(title) // "front-end-interview"
```

正如你所看到的，`compose` 函数的作用就是将两个函数合成为一个。`compose` 将两个函数串联起来执行，`toLowerCase` 函数返回的结果会传递给下一个函数 `addHyphens` 中作为参数。

另一个示例：

```js
// 从右到左组合函数
const compose =
  (...fns) =>
  (x) =>
    fns.reduceRight((y, f) => f(y), x)

const lowercase = (str) => str.toLowerCase()
const capitalize = (str) => `${str.charAt(0).toUpperCase()}${str.slice(1)}`
const reverse = (str) => str.split('').reverse().join('')

const fn = compose(reverse, capitalize, lowercase)

// 我们将按顺序执行 lowercase，capitalize 和 reverse
fn('Hello World') //  'dlrow olleH'
```

## 更多资料

- [JavaScript 专题之函数组合](https://github.com/mqyqingfeng/Blog/issues/45)
- [实现 compose 的五种思路](https://segmentfault.com/a/1190000011447164)
