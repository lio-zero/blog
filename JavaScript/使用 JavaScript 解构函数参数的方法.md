# 使用 JavaScript 解构函数参数的方法

使用对象解构和 `Object.assign()` 方法将大量参数传递给函数。

```js
function foo(args) {
  // 获取参数值
  let { name, age, job, flag } = Object.assign(
    {
      age: null,
      job: 'Programmer',
      flag: false
    },
    args
  )

  console.log(name, age, job, flag)
}
```

您还可以通过直接在参数赋值中解构来获得相同的结果。甚至可以指定默认参数值。

```js
function foo({ name, age, job = 'Programmer', flag = false } = {}) {
  console.log(name, age, job, flag)
}
```

无论采用哪种方法，您都可以像这样传递您的参数。

```js
foo({
  name: 'O.O',
  age: 20
}) // "O.O", "20", "Programmer", false
```

就个人而言，我觉得使用 `Object.assign()` 更容易扫描和阅读，因为每个参数都在自己的行中。

但它们都有相同的功能，所以选择你觉得更容易阅读和使用的。
