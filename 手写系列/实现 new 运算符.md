# 实现 new 运算符

[`new`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/new) 运算符可以帮助我们创建一个由构造函数定义的对象的实例，并且绑定上 `this`。

## 原理

MDN 介绍了 `new` 运算符执行的操作步骤如下：

1. 创建一个空的 JavaScript 对象（即 `{}`）；
2. 为步骤 1 新创建的对象添加属性 `__proto__`，将该属性链接至构造函数的原型对象（ `constructor.prototype`）；
3. 将步骤 1 新创建的对象作为 `this` 的上下文 ；
4. 如果该函数没有返回对象，则返回 `this`。

> [地址](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/new)。

## 实现

根据上述所描述的，我们可以模拟出一个 `new` 运算符。

```js
function myNew() {
  // 步骤一：创建一个新对象。
  let obj = {}

  // 步骤二：将新对象原型（__proto__）设置为构造函数的原型对象。
  let Constructor = [].shift.call(arguments) // 使用 shift 方法从参数数组中删除并返回第一个元素，即构造函数。
  obj.__proto__ = Constructor.prototype

  // 步骤三：使用 apply 方法将 this 指向从 Constructor 转为新对象 obj，并执行构造函数。
  let ret = Constructor.apply(obj, arguments)

  // 步骤四：如果构造函数返回一个对象，则返回该对象，否则返回新对象。
  return {}.toString.call(ret) === '[object Object]' ? ret : obj
}
```

示例：

```js
function Person(name, age) {
  this.name = name
  this.age = age
}

let p = myNew(Person, 'O.O', 20)
console.log(p) // { name: 'O.O', age: 20 }
```

> **扩展**：[`new.target`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/new.target) 属性可以检测函数或构造方法是否是通过 `new` 运算符被调用的。

你还可以使用 `Object.create()` 方法简化步骤一和步骤二的实现过程。

## 扩展：可以使用 `new` 一个箭头函数吗？

箭头函数是没有自己的 `this` 绑定的，它的 `this` 绑定是继承自其所在的作用域的，而 `new` 运算符会为构造函数创建一个新的 `this` 绑定。因此，尝试使用 `new` 运算符调用箭头函数会导致错误。

> 箭头函数也没有 `prototype` 和 `arguments`。

## 更多资料

- [第 14 题：情人节福利题，如何实现一个 new](https://github.com/Advanced-Frontend/Daily-Interview-Question/issues/12)
- [JavaScript 深入之 new 的模拟实现](https://github.com/mqyqingfeng/Blog/issues/13)
- [JS 的 new 到底是干什么的？](https://zhuanlan.zhihu.com/p/23987456)
