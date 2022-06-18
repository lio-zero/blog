# 如何在 JavaScript 中选择或忽略对象的属性

在没有内置解决方案的情况下，从 JavaScript 对象中选择或忽略属性是一个相当常见的问题。

下面我们来看看，实现两个方法 `choice` 和 `ignore` 来选择或忽略对象的属性。

## 从对象中选择属性

如果要从 JavaScript 对象中选择任意数量的属性，实现如下：

```js
const choice = (obj, ...props) => {
  return props.reduce((result, prop) => {
    result[prop] = obj[prop]
    return result
  }, {})
}
```

`choice` 函数的第一个参数将是我们要从中选择的对象，随后的参数将是我们要保留的键的名称。

```js
const user = {
  name: 'IU',
  age: 18,
  gender: '女'
}

console.log(choice(user, 'name', 'age')) // { name: "IU", age: "18" }
```

可以看到，通过提供 `user` 对象作为第一个参数，然后提供字符串 `name` 和 `age` 作为后续参数，我们可以保留对象中的`name` 和 `age` 属性，而忽略 `gender` 属性。

## 忽略对象的属性

如果要从 JavaScript 对象中省略任意数量的属性，实现如下：

```js
const ignore = (obj, ...props) => {
  const result = { ...obj }
  props.forEach((prop) => delete result[prop])
  return result
}
```

同样，让我们使用相同的 `user` 对象来查看实际效果。

```js
const user = {
  name: 'IU',
  age: 18,
  gender: '女'
}

console.log(ignore(user, 'age')) // { name: "IU",  gender: "女" }
```

可以看到，通过提供 `user` 对象作为第一个参数，字符串 `age` 作为第二个参数，我们能够获得一个省略了 `age` 省略属性的新对象。
