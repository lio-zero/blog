# JS 作用域和作用域链

## 作用域

> JavaScript 中的作用域是我们可以有效访问变量或函数的区域。作用域规定了如何查找变量，也就是确定当前执行代码对变量的访问权限。

JavaScript 有两种类型的作用域：全局作用域、局部作用域。局部作用域又可以分为：函数作用域、块作用域（ES6）和其他更具体的作用域。

### 全局作用域

```js
let age = 18

function foo() {
  // 不是嵌套函数
  // 函数内部可以访问函数外部变量
  return age
}

foo() // 18
```

### 函数作用域

```js
function text() {
  var age = 18
}

// 函数外部访问不到函数内部变量
console.log(age) // ReferenceError
```

### 块作用域

```js
{
  let age = 18
}

// let、const 关键字声明的变量，只在其所在的代码块 {} 内有效。
console.log(age) // error
```

## 词法作用域和动态作用域

> **词法作用域**是在定义时确定的，它关注函数在何处声明。
>
> **动态作用域**是在运行时确定的（`this` 也是），它关注函数从何处调用（常与 `this` 挂钩）。

JavaScript 采用的是词法作用域，又称静态作用域。下面我们来看看示例：

```js
var value = 1

function foo() {
  console.log(value)
}

function bar() {
  var value = 2
  foo()
}

bar() // 1
```

分析：

如果是词法作用域，调用 `foo` 函数时，会先在其内部作用域中查找，如果找不到，往上一层查找，找到了 `value`，结果为 1。

如果是动态作用域，调用 `foo` 函数时，同样会先在其内部作用域中查找，如果找不到，会到调用 `foo` 函数的作用域中查找（这里是 `bar`），结果为 2。

## 作用域链

一般情况下，变量取值应到创建这个变量的函数的作用域中取值。如果在当前作用域找不到，向上级作用域查找，直到全局作用域，这么一个查找过程形成的链条就叫做作用域链。

1. 每当编译器遇到变量或对象时，它都会遍历当前执行上下文的整个作用域链（Scope chain），以查找它。
2. 如果没有在那里找到它，它遍历原型链（prototype chain），如果它也没有找到，它会抛出未定义的错误。
3. 编译器通过查看函数在代码中的位置来创建函数的作用域。
4. 编译器创建称为作用域链（Scope chain） 的等级层次结构，全局作用域（Global Scope） 位于此层次结构的顶部。
5. 当代码中使用变量时，编译器会向后查看作用域链，如果找不到，则抛出未定义的错误。

## 更多资料

- [JavaScript 深入之词法作用域和动态作用域](https://github.com/mqyqingfeng/Blog/issues/3)
- [JavaScript 深入之作用域链](https://github.com/mqyqingfeng/Blog/issues/6)
- [变量作用域，闭包](https://zh.javascript.info/closure)
- [ES6: var, let and const — The battle between function scope and block scope](https://www.deadcoderising.com/2017-04-11-es6-var-let-and-const-the-battle-between-function-scope-and-block-scope/)
- [You Don't Know JS Yet: Scope & Closures - 2nd Edition](https://github.com/getify/You-Dont-Know-JS/tree/2nd-ed/scope-closures)
