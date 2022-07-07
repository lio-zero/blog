# JavaScript 立即执行函数表达式（IIFE）

**立即执行函数表达式**（IIFE，immediately invoked function expression）是一个立即执行的匿名函数表达式，它有两种写法：

```js
;(function () {
  'use strict'
  // ...
})()

// or

;(function () {
  'use strict'
  // ...
} ())

// 上面两种都等价于命名函数
function sayHello() { ... }
sayHello()

// 你还在可以在 IIFE 中使用箭头函数
;(() => {})()
```

可以看到，**立即执行函数**有两个括号组成：一个括号用于包裹匿名函数（使其成为一个函数表达式），一个括号用于立即执行函数。

如果你看过一些很早之前的库，例如 jQuery 的源码，你应该知道下面这种情况：

```js
+(function ($) {})(window.jQuery)
```

我们还可以向第二个括号添加一些参数，来作为匿名函数的参数，一旦 JavaScript 运行时对其求值，它就会立即被调用：

```js
// IIFE
;(function (message) {
  console.log(message) // 'Hello World'
})('Hello World')

// 使用命名函数的等价代码
function sayHello(message) {
  console.log(message)
}

sayHello('Hello World') // 'Hello World'
```

由于有些人习惯在语句结束后不加分号，如下面的 `var a = 1` 语句没有添加分号，所以该片段将报错。

```js
// 出现错误的情况
var a = 1
(function () {
  console.log('Hello')
})()
// Uncaught TypeError: 1 is not a function
```

你应该在立即执行函数前添加**分号或感叹号**等，来避免在某些情况下出现 `TypeError`。

```js
// 所有这些都是等价的
;(function(){ console.log('Hello') })()
!(function(){ console.log('Hello') })()
+(function(){ console.log('Hello') })()
-(function(){ console.log('Hello') })()
~(function(){ console.log('Hello') })()
// ...
```

IIFE 通常用于创建一个独立的作用域，运行某些代码，同时将所有变量和函数保持在全局范围之外以避免冲突时使用。

它还常用于经典的[闭包](https://github.com/lio-zero/blog/blob/master/JavaScript/JavaScript%20%E4%B8%AD%E7%9A%84%E9%97%AD%E5%8C%85.md)：

```js
for (var i = 1; i <= 10; i++) {
  ;(function (j) {
    setTimeout(function () {
      console.log(j)
    }, 1000)
  })(i)
}

// 额外的提示1：使用 ES6 的 let 或 const 可以轻松解决
for (let i = 1; i <= 10; i++) {
  setTimeout(function () {
    console.log(i)
  }, 1000)
}

// 额外的提示2：setTimeout 的第三个参数
for (let i = 1; i <= 10; i++) {
  setTimeout(function (i) {
    console.log(i)
  }, 1000, i)
}
```

IIFE 并都不是 JS 的规范，其为早期社区一些能人异士发现，并广泛推广。而现在有 ES6 的块级作用域、模块系统和 Transpiler 的兴起，IIFE 已经不在推荐使用。
