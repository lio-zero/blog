# 实现一个 add 方法

实现一个 `add` 方法，预期效果如下：

```js
add(1)(2, 3)(4).value() // 10
```

实现如下：

```js
function add(...args) {
  const result = add.bind(null, ...args)
  // const result = (...args1) => add(...args, ...args1)
  result.value = () => args.reduce((acc, cur) => acc + cur)
  return result
}

add(1)(2, 3)(4).value() // 10
add(1)(2)(3)(4).value() // 10
add(1, 2, 3)(4).value() // 10
```

我们使用了 Rest 运算符将传递给 `add` 函数的多个参数绑定到一个数组中。接着，我们使用函数的 `bind` 方法来创建一个新的函数，并通过 Spread 运算符将参数传递给新函数。

为新创建的函数添加一个 `value` 属性，该属性是一个函数，可以返回所有参数的总和。我们可以使用 `reduce` 方法来计算参数的总和。

## 更多资料

- [请实现一个 add 函数，满足以下功能](https://github.com/Advanced-Frontend/Daily-Interview-Question/issues/134)
- [柯里化](https://github.com/lio-zero/blog/blob/master/JavaScript/%E6%9F%AF%E9%87%8C%E5%8C%96.md)
