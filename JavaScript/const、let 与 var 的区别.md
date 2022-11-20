# const、let 与 var 的区别

JS 有三个关键字可用于声明变量：

- `var`
- ES6 新增的 `let` 和 `const`

## 区别

无法在声明 `let` 变量的最近封闭块（块级作用域）之外访问该变量。

```js
{
  let foo
}

// ReferenceError: foo is not defined
console.log(foo)
```

如果我们用 `var` 声明替换 `let`，上面的示例代码就可以工作。

- `let` 变量在声明之前不能使用。下面的示例代码将引发 `ReferenceError`：

```js
var foo = 'hello'
{
  console.log(foo)
  let foo = 'hi'
}
// ReferenceError: Cannot access 'foo' before initialization
```

如果在上面的示例代码中使用 `var`，我们将在控制台中看到 `hello`。因为 `var` 存在变量提升，而 `let` 存在 TDZ（Temporal Dead Zone，暂存性死区），即只要块级作用域中存在 `let`，那么它所声明的变量就绑定了这个区域，不再受外部的影响。

- 同一作用域下 `let` 不能声明同名变量。

```js
// var 可以使用相同的名称重新声明变量
var foo, foo
var bar = 1
var bar
bar // 1

// let 则不行
let baz
let baz
// 抛出以下错误
// SyntaxError: Identifier 'baz' has already been declared
```

- 在顶层，全局 `let` 变量不会附加到全局 `window` 对象。

```js
var bar = 'world'
window.bar // 'world'

let foo = 'hello'
window.foo // undefined
```

- 使用 `let` 可以解决 `var` 无法解决的闭包问题。

```js
for (var i = 0; i < 3; i++) {
  setTimeout(function () {
    console.log(i) // 3 3 3
  }, i * 1000)
}
```

它不像我们期望的那样工作。`for` 是同步任务，一下子就运行完了，`setTimeout` 属于异步任务，回调中的变量 `i` 将引用同一个对象。最终 `for` 循环到 `i = 3` 停止循环，而这个 `i` 会被赋予到顶层 `window` 对象上，所以最终结果输出的都是 3。

我们可以使用 `let` 解决此问题：

```js
for (let i = 0; i < 3; i++) {
  setTimeout(function () {
    console.log(i) // 0 1 2
  }, i * 1000)
}
```

- `const` 关键字的行为与 `let` 相同，只是在声明时必须赋予一个初始值。

```js
// 抛出以下错误
// SyntaxError: Missing initializer in const declaration
const a
```

且变量不能更改：

```js
const a = 'hello'

// 抛出以下错误
// TypeError: Assignment to constant variable
a = 'world'
```

**值得注意的是**，使用 `const` 并不意味着变量是不可变的。可以更改对象的属性：

```js
const person = {}
person.age = 20

// 向数组中添加更多项
const arr = []
arr.push('foo')
arr[1] = 'bar'
console.log(arr) // ['foo', 'bar']
```

## 建议

- 不要使用 `var`，除非您必须支持那些不支持 `let` 和 `const` 关键字的旧浏览器。
- [关于使用 `let` 还是 `const` 的一些讨论](https://overreacted.io/on-let-vs-const/)

## 总结

- 声明变量的三种方式：`var`、`let`、`const`。
- `var` 和 `let` 用于声明变量，而 `const` 用于声明只读的常量。
- `var` 在全局作用域下声明变量，全局可用，而在函数作用域下声明的变量，只能在当前的环境下使用。
- `var` 声明的变量，不是全局作用域就是函数作用域，没有块级作用域，会忽略代码块 `{}`（除函数外），其因在于 JS 早期块没有词法环境。
- `var` 声明的变量允许重新声明，而 `let` 和 `const` 在相同作用域内，不允许重新声明。
- `var` 声明的变量存在变量提升，但是赋值不会。`let` 和 `const` 不存在变量提升。
- `let` 和 `const` 声明形成块作用域，声明的变量只能在它声明的封闭块内使用。
- 同一作用域下 `let` 和 `const` 不能声明同名变量，而 `var` 可以。
- `let` 和 `const` 存在暂存性死区（TDZ）。
- `let` 和 `const` 的唯一区别是：`const` 变量一旦声明必须赋值，声明后不能再修改，但如果声明的是对象，我们可以更改其属性。
