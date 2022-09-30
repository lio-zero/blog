# JavaScript 严格模式（'use-strict'）

[严格模式](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Strict_mode)（`'use strict'`）是 ES5 中的一个新特性，它允许您将程序或函数置于“严格”操作上下文中。这种严格的上下文阻止执行某些操作，并引发更多异常。

您可以对整个文件应用“严格模式”。或者，将其用于特定函数：

```js
'use strict'

// Non-strict code...
;(function () {
  'use strict'

  // Define your library strictly...
})()

// Non-strict code...
```

> **Tips**：ES6 的原生模块（`import` 和 `export`）和类（`class`）默认启动严格模式。

## 设立 "严格模式" 的目的

严格模式有以下几种帮助：

- 消除 JavaScript 语法的一些不合理、不严谨之处，减少一些怪异行为。
- 当采取相对“不安全”的操作（例如访问全局对象）时，它可以防止或引发错误。
- 提高编译器效率，增加运行速度。
- 它捕获一些常见的编码错误，通过**抛出异常**来消除了一些原有**静默错误**。
- 它会禁用令人困惑或考虑不周的功能，包括在 ECMAScript 的未来版本中可能会定义的一些语法。

严格模式主要删除了 ES3 中可能的功能，并且自 ES5 以来已被弃用（但由于向后兼容性要求而未被删除）

IE6-9 不支持严格模式，详细的支持情况如下：

![严格模式](https://upload-images.jianshu.io/upload_images/18281896-182e8f905fe2a70c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 严格模式主要有以下限制

- 变量必须声明后再使用

```js
'use strict'
x = 1 // Error（x 未定义）
```

在函数内部声明是局部作用域（只在函数内使用严格模式）

```js
x = 1

func()
function func() {
  'use strict'
  y = 2 // Error
}
```

- 函数的参数不能有同名属性，否则报错

```js
'use strict'
function func(x, x) {} // Error
```

- 在带有默认参数的函数中不允许使用**严格模式**

```js
function func(a = 1, b = 2) {
  'use strict'
  return a + b
} // SyntaxError
```

- 不能使用 `with` 语句

```js
'use strict'
with (Math) {
  x = cos(2)
} // SyntaxError
```

- 不能对只读属性赋值，否则报错

```js
'use strict'
let obj = {}
Object.defineProperty(obj, 'x', {
  value: 0,
  writable: false
})

obj.x = 1 // TypeError
```

- 不能使用前缀 0 表示八进制数，否则报错

```js
'use strict'
var x = 010 // SyntaxError

// 它有时会令人混淆

// 但您仍然可以使用以下 0oXX 语法在严格模式下启用八进制数：
0o10
```

- 不能删除不可删除的属性，否则报错

```js
'use strict'

delete Object.prototype // TypeError
```

- 不能删除变量 delete prop，会报错，只能删除属性 `delete global[prop]`

```js
'use strict'

let obj = {
  name: 'lio'
}

delete obj // SyntaxError
delete obj[name] // √
```

- 由于一些安全原因，在作用域 `eval()` 创建的变量不能被调用

```js
'use strict'

eval('var x = 2')
console.log(x) // ReferenceError
```

其他：

- `eval` 和 `arguments` 不能被重新赋值
- `arguments` 不会自动反映函数参数的变化
- 不能使用 `arguments.callee` 和 `arguments.caller`
- 禁止 `this` 指向全局对象
- 不能使用 `fn.caller` 和 `fn.arguments` 获取函数调用的堆栈
- 增加了保留字（比如 `protected`、`static` 和 `interface` 等）

## 更多资料

[What does "use strict" do in JavaScript, and what is the reasoning behind it?](https://stackoverflow.com/questions/1335851/what-does-use-strict-do-in-javascript-and-what-is-the-reasoning-behind-it)
