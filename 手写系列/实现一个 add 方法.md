# 实现一个 add 方法

实现一个 add 方法，预期效果如下：

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
```

## 更多资料

[柯里化](https://github.com/lio-zero/blog/blob/master/JavaScript/%E6%9F%AF%E9%87%8C%E5%8C%96.md)
