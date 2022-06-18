# JavaScript 函数方法 — call()，apply() 和 bind()

## Function.prototype.call()

[`Function.prototype.call()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/call) 用于调用具有给定 `this` 上下文和单独提供的任何参数的函数。例如：

```js
function getUserInfo(...data) {
  console.log(this.data, ...data)
}

const user = {
  name: 'IU',
  age: 18,
  love: 'me'
}
const data = [0, 1, 2]

getUserInfo.call(user, data) // IU [0, 1, 2]
getUserInfo.call(user, ...data) // IU 1 2 3
```

## Function.prototype.apply()

[`Function.prototype.apply()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/apply) 与 `Function.prototype.call()` 几乎相同，因为它使用给定的 `this` 上下文调用函数，但它要求参数作为数组提供。例如：

```js
function getUserInfo(...data) {
  console.log(this.name, ...data)
}

const user = {
  name: 'IU',
  age: 18,
  love: 'me'
}
const data = [0, 1, 2]

getUserInfo.apply(user, data) // IU [0, 1, 2]
getUserInfo.apply(user, ...data) // 抛出 TypeError
```

## Function.prototype.bind()

[`Function.prototype.bind()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/bind) 与前两种方法略有不同。它不是使用给定的 `this` 上下文调用函数并返回结果，而是返回一个绑定了 `this` 上下文的函数，以及在调用返回的函数时在参数前面单独提供的任何参数。例如：

```js
const user = {
  name: 'IU',
  age: 18,
  love: 'me'
}
const data = [0, 1, 2]

function getUserInfo(...data) {
  console.log(this.name, ...data)
}

let boundGetUserInfo = getUserInfo.bind(user)

boundGetUserInfo(data) // IU [0, 1, 2]
boundGetUserInfo(...data) // IU 1 2 3

const boundGetUserInfo2 = getUserInfo.bind(user, 2)

boundGetUserInfo2(data) // IU 2 [0, 1, 2]
boundGetUserInfo2(...data) // IU 2 1 2 3
```

## 总结

以下是 `call` 和 `apply` 方法的语法，其中 `fn` 表示给定函数：

```js
fn.apply(context, arrayOfArguments)
fn.call(context, arg1, arg2, ...)
```

- `call` 和 `apply` 都用于调用函数，第一个参数将用作函数内 `this` 的值。
- `call` 接受逗号分隔的参数作为后面的参数，而 `apply` 接受一个参数数组作为后面的参数。

例如，下面的函数返回三个数字的和。

```js
const sum = (a, b, c) => a + b + c

sum.apply(null, [1, 2, 3]) // 6
sum.call(null, 1, 2, 3) // 6
```

- 您可以使用标记方法来记住 `apple`（A 代表数组）和 `call`（C 代表逗号）之间的区别。
- `call` 和 `apply` 的第一个参数传入 `null` 时，它在调用时将被忽略，实际应用的是默认绑定规则：

```js
var count = 3
function add(a, b) {
  console.log(this.count, a, b)
}

add.call(null) // 3 undefined undefined
add.call(null, 1, 2) // 3 1 2
add.apply(null, [1, 2]) // 3 1 2
```

- `bind` 返回一个绑定了 `this` 上下文的函数，它不像 `call` 和 `apply` 一旦使用立即执行，你可以在之后执行这个返回的函数，它的参数和 `call` 一样，并且可以将参数在执行的时候添加。
