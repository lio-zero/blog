# 为什么递归函数中返回结果为 undefined？

在掌握递归的诀窍时，一个常见的问题是不理解递归函数返回 `undefined` 的原因。

通常，这是因为您没有 `return` 递归函数的下一次执行。

我们来写一个递归阶乘函数。例如，5 的阶乘（写成 5!）等于 `5 * 4 * 3 * 2 * 1 = 120`。

```js
const factorial = (num, result = 1) => {
  if (num === 1) return result
  factorial(num - 1, num * result)
}

console.log(factorial(120)) // undefined
```

可以看到，我们想要的结果应该是 `120`，但返回了 `undefined`，为什么呢？其实很简单，我们没有将 `factorial` 函数 `return` 出去执行下一次的递归调用。

```js
const factorial = (num, result = 1) => {
  if (num === 1) return result
  return factorial(num - 1, num * result)
}

console.log(factorial(5)) // 120
```

就这样，不管函数怎么走，都要确保 `return` 出一些东西。不然它将默认返回 `undefined`。

## 更多资料

[JavaScript 专题之递归](https://github.com/mqyqingfeng/Blog/issues/49)
