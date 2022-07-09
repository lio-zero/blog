# JavaScript 中定义函数的方法

通常，函数是一系列指令或“子程序”，可由该函数外部（或内部）的代码调用。本质上，函数“封装”了一个特定的任务。

函数是 JavaScript 的基本构建块之一，真正理解函数有助于解决 JavaScript 的一些奇怪的行为。

以下是函数可视化图（来自...忘了，找找）：

![JS 函数可视图](https://upload-images.jianshu.io/upload_images/18281896-fe7fd7cf900b314d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## JavaScript 中的函数

需要注意的是，JavaScript 中的函数是一等对象。这基本上意味着 JavaScript 中的函数可以像对待任何其他 JavaScript 对象一样对待，可以作为其他变量引用或作为参数传递给函数。

函数甚至可以具有属性和其他方法，就像任何其他 JavaScript 对象一样。函数和其他对象之间的关键区别在于可以调用函数。

JavaScript 中的每个函数都是一个 `Function` 对象。尝试在控制台执行：

```js
function typeCheck() {}

typeCheck instanceof Function // true
```

`Function` 对象有一些特定的方法和属性，如 `apply`、`call`、`bind` 和 `isGenerator` 等，这些方法和属性在其他对象中是不可用的。

有几种不同的方式可以在 JavaScript 中定义函数，定义函数的方式会影响函数行为。

### 函数声明

这可能是定义函数最常见的方法。函数声明包含一个名称，前面是强制 `function` 关键字，后面是所需括号 `()` 内的可选参数列表。

```js
function sum(param1, param2) {
  return param1 + param2
}
```

**关于这种形式的函数定义，有两点需要注意**：

- 保存函数对象的变量在当前作用域中创建，其标识符与提供的函数名相同 —— 在我们的示例中为 `sum`。
- 变量被**提升**到当前作用域的顶部。

为了更好地理解提升，让我们看一个例子：

```js
sayHi() // 'Hi'

function sayHi() {
  return 'Hi'
}
```

我们能够在定义之前调用 `sayHi` 函数。

还有一点：在非严格模式下，函数允许我们使用重复的命名参数。

```js
function double(x, x) {
  return x + x
}

double(3, 2) // 4
double(3, 5) // 10
```

在严格模式下：

```js
'use strict'
function double(x, x) {}

// ERROR: Duplicate parameter name not allowed
```

### 函数表达式

函数表达式在语法上与函数声明非常相似。主要区别在于函数表达式不需要函数名。

```js
let sum = function (param1, param2) {
  return param1 + param2
}
```

函数表达式是另一个语句的一部分。在上面的例子中，函数表达式是 `sum` 变量赋值的一部分。

与函数声明不同，函数表达式不会被提升。

```js
sayHi() // Uncaught ReferenceError: sayHi is not defined

let sayHi = function () {
  return 'Hi'
}
```

函数表达式的一个有趣用例是它们能够创建 IIFE（立即执行函数表达式）。在某些情况下，我们可能想要定义一个函数并在定义后立即调用它，但不会再次调用。

当然，它可以通过函数声明来完成，但是为了使它更具可读性，并确保我们的程序不会意外地访问它，我们使用了 IIFE。考虑这个例子：

```js
function callImmediately(foo) {
  console.log(foo)
}
```

我们创建了一个名为 `callImmediately` 的函数，它接受一个参数并打印它，然后我们立即调用它。通过这样做可以获得相同的结果：

```js
;(function (foo) {
  console.log(foo)
})('foo') // 'foo'
```

关键的区别在于，在第一种情况下，函数声明会污染全局命名空间，并且命名函数 `callImmediately` 需要它之后很长时间就挂起了。IIFE 是匿名的，因此将来无法调用。

> 推荐：[JavaScript 立即执行函数表达式（IIFE）](https://github.com/lio-zero/blog/blob/main/JavaScript/JavaScript%20%E7%AB%8B%E5%8D%B3%E6%89%A7%E8%A1%8C%E5%87%BD%E6%95%B0%E8%A1%A8%E8%BE%BE%E5%BC%8F%EF%BC%88IIFE%EF%BC%89.md)

### 箭头函数

箭头函数是 ES6 的一个补充，是函数表达式的语法紧凑的替代品。箭头函数是使用一对包含参数列表的圆括号定义的，后跟一个 `=>` 和带有大括号 `{}` 的函数语句。

```js
let sum = (param1, param2) => {
  return param1 + param2
}
```

由于箭头函数主要目的是语法紧凑性，如果箭头函数中唯一的语句是 `return`，我们可以删除大括号和 `return` 关键字，如下所示：

```js
let sum = (param1, param2) => param1 + param2
```

它使代码更具可读性。

此外，如果我们只有一个参数传递给箭头函数，则可以消除括号：

```js
let double = param1 => param1 * 2
```

**关于这种形式的函数定义，有几点需要注意**：

- 箭头函数没有自己的 `this`，它使用封闭词法作用域的 `this` 值。

```js
let user = {
  name: 'O.O',
  getUserNameArrow: () => {
    console.log(this.name)
  },
  getUserNameExpression: function () {
    console.log(this.name)
  }
}
user.getUserNameArrow() // ''
user.getUserNameExpression() // O.O
```

在上面的示例中，我们有一个箭头函数和一个函数表达式，它使用 `this` 函数打印 `user.name`。

- 箭头函数没有 `prototype` 属性。

```js
let foo = () => {}
console.log(foo.prototype) // 'undefined'
```

- 箭头函数中没有自己的 `arguments` 对象。在箭头函数中访问 `arguments` 对象实际上是获取的是外部函数执行环境中的值。

```js
function add(a, b) {
  console.log('outer', ...arguments)
  return (argument) => {
    console.log('inner', ...arguments)
  }
}
add(1, 2)()
// outer 1 2
// inner 1 2
```

- `call`、`apply` 和 `bind` 不会影响箭头函数 `this` 值。

```js
const a = () => {
  console.log(this)
}

const b = function () {
  console.log(this)
}

const user = { name: 'O.O' }

a.call(user) // window { ... }
b.call(user) // {name: 'O.O'}
```

- 箭头函数不能当作 `Generator` 函数，不能使用 `yield` 关键字。
- 箭头函数不能使用 `new` 关键字，箭头函数没有 `prototype`、没有自己的 `this` 指向、不可以使用 `arguments`，自然不可以 `new`。
- 箭头函数中的不同参数不能使用相同的名称，无论启用严格模式还是非严格模式。

### `Function` 构造函数

前面说过，JavaScript 中的每个函数都是 `Function` 对象，因此要定义函数，我们还可以直接调用 `Function` 对象的构造函数。

```js
let sum = new Function('param1', 'param2', 'return param1 + param2')
```

参数以逗号分隔字符串 `'param1', 'param2', ..., 'paramN'` 的列表形式传递，最后一个参数是以字符串形式传递的函数体。

就性能而言，这种定义函数的方法不如函数声明或函数表达式有效。每次调用构造函数时，都会解析使用 `Function` 构造函数定义的函数，因为每次都需要解析函数体字符串，而其他函数体字符串则与其余代码一起解析。

以这种方式定义函数的一个用例是访问 Node 中的 `global` 对象或浏览器中的 `window` 对象。这些函数始终在全局作用域中创建，并且无权访问当前作用域。

### Generator 函数

`Generator` 是 ES6 的补充，译为**生成器**。生成器是一种特殊类型的函数，与传统函数不同，生成器在每个请求的基础上生成多个值，同时在这些请求之间暂停执行。

```js
function* idMaker() {
  let index = 0
  while (true) yield index++
}

let gen = idMaker()

console.log(gen.next().value) // 0
console.log(gen.next().value) // 1
console.log(gen.next().value) // 2
```

`function*` 和 `yield` 关键字对于生成器是唯一的。生成器是通过在函数关键字末尾添加 `*` 来定义的。这使我们能够在生成器主体中使用 `yield` 关键字来根据请求生成值。

## 结论

选择使用哪种定义类型取决于情况和您要实现的目标。记住的几个基本要点：

- 如果您想利用函数提升，请使用函数声明。例如，为了清晰起见，您希望将函数实现细节移到底部，而只将抽象流程移到顶部。
- 箭头函数非常适合于短回调函数，更重要的是，当需要的 `this` 是封闭函数时。
- 避免使用 `Function` 构造函数来定义函数。如果烦人的语法还不足以让您远离，那么它会非常慢，因为每次调用该函数时都会对其进行解析。

## 更多资料

关于函数内部 this 的区别，可以阅读 [JavaScript 中的 “this” 关键字](https://github.com/lio-zero/blog/blob/main/JavaScript/JavaScript%20%E4%B8%AD%E7%9A%84%20%E2%80%9Cthis%E2%80%9D%20%E5%85%B3%E9%94%AE%E5%AD%97.md)
