# variable === undefined 与 typeof variable === undefined 的区别

检查变量是否为 `undefined` 有两种常用方法。我们可以使用相等运算符（`==`）或 `typeof` 运算符：

```js
variable === undefined
typeof variable === 'undefined'
```

## 区别

`typeof` 运算符适用于未声明的变量，而相同的运算符将引发 `ReferenceError` 异常。

```js
typeof undeclaredVar === 'undefined' // true
undeclaredVar === undefined // 引发 ReferenceError 异常
```

## 提示

在运行 ES3 enginee 的旧浏览器中，`undefined` 是一个全局变量名，其原始值未定义。可以更改该值：

```js
// ES3
var person = {}
person.name === undefined // true

// 让我们修改 undefined 的值
undefined = 'Foo'
person.name === undefined // false
```

为了避免未定义的可重命名或修改值的问题，我们可以将代码包装在 IFFE（立即调用的函数表达式）中，如下所示：

```js
;(function (undefined) {
  // 在这里使用 undefined 是安全的
})()

// Or
;(function (undefined) {
  // ...
})(_)
```

在上面的示例代码中，`undefined` 是函数的一个参数。由于我们不向函数传递任何参数或未定义的变量（`_`），因此该参数将是未定义的。这种常见模式用于流行的库，如 [jQuery](https://jquery.com/)、[Backbone](https://backbonejs.org/) 等。

如今的现代浏览器并非如此。从 ES5 中，无法更改 `undefined`，因为其 `Writable` 属性设置为 `false`。

> 建议始终使用 [`typeof` 运算符](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/typeof)检查操作数的类型。
