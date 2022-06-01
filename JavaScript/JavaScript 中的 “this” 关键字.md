# JavaScript 中的 “this” 关键字

在 JavaScript 中，`this` 关键字是指当前正在运行代码时所在的环境对象。

`this` 的值取决于函数的调用方式：

- 默认情况下，指全局对象。
- 在函数中，当不处于严格模式时，它引用全局对象 `window`；当处于严格模式时，它将是 `undefined`。
- 在箭头函数中，`this` 保留封闭词法上下文的 `this` 值。
- 在对象方法中，`this` 指调用该方法的对象。
- 在构造函数调用中，`this` 绑定到正在构造的新对象。
- 在事件处理程序中（DOM 事件绑定），`this` 绑定到监听器所在的元素。
- `call/apply/bind` 可以显式绑定 `this` 的指向

## 全局上下文

在全局执行上下文中，`this` 指的是全局对象（浏览器中的 `window`）。

```js
console.log(this === window) // true
```

## 函数上下文

在非严格模式下，函数的 `this` 引用全局对象。

```js
function func() {
  return this
}

console.log(func() === window) // true
```

在严格模式（`'use strict'`）下，如果在进入执行上下文时未设置，则函数的 `this` 将以 `undefined` 代替全局对象。

```js
'use strict'
function func() {
  return this
}

func() // undefined
```

## 构造函数

如果在调用函数时使用 `new` 关键字，那么函数中的 `this` 指的是一个全新的对象。

```js
function User() {
  return this
}
User() // 返回 window 对象

function User(name) {
  this.name = name
}

new User('lio') // User {name: "lio"}
new User('lion') // User {name: "lion"}
```

可以看看 `new` 的原理。

## 对象上下文

如果函数作为一个对象的方法调用，则 `this` 是指该方法被调用的对象。

```js
const user = {
  name: 'lio',
  age: 18,
  run() {
    return this.name
  }
}

user.run() // "lio"
```

这适用于在对象的原型链的任何地方定义的方法（即自己的和继承的方法）。

## 箭头函数上下文

如果是 ES6 的箭头函数，在箭头函数内部访问到的 `this` 都是从外部获取的。`this` 值取决于外部不是箭头函数的函数。

```js
const func = () => this

console.log(func() === window) // true

const user = {
  name: 'lio',
  age: 18,
  run() {
    const getUser = () => this.name
    return getUser()
  },
  foo: () => this
}

console.log(user.run()) // lio
console.log(user.foo() === window) // true
```

> **注意**：箭头函数没有自己的 `this`，`arguments`，`super` 等。

## 事件处理程序上下文

在事件处理程序中使用时，它指的是放置监听器的元素。

```js
btn.addEventListener('click', function () {
  console.log(this) // 返回 <button> DOM 对象
})

// 使用箭头函数返回 window 全局对象
btn.addEventListener('click', () => {
  console.log(this) // 返回 window 全局对象
})
```

## call/apply/bind 绑定

如果使用 `apply`、`call` 或 `bind` 来调用/创建函数，则函数中的对象就是作为参数传入的对象。

使用 `Function.prototype.bind()` 从现有函数返回一个新函数，其中该函数永久绑定到 `bind()` 的第一个参数。

```javascript
function func() {
  console.log(this)
}

const user = {
  name: 'lio',
  age: 18
}

const v = func.bind(user)
v() // {name: "lio", age: 18}
```

使用 `Function.prototype.call()` 或 `Function.prototype.apply()` 将被调用函数的 `this` 绑定到这两个函数的第一个参数。

```js
function func() {
  console.log(this)
}

const user = {
  name: 'lio',
  age: 18
}

func() // Window { ... }
func.apply(user) // {name: "lio", age: 18}
```

## 更多资料

[Understanding the "this" keyword in JavaScript](https://www.30secondsofcode.org/blog/s/javascript-this)
