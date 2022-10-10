# 实现 new 运算符

[`new`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/new) 运算符创建一个用户定义的对象类型的实例或具有构造函数的内置对象类型之一。

`new` 操作符可以帮助我们构建出一个实例，并且绑定上 `this`。

## 原理

> MDN 对 **`new`** 解释的操作步骤如下：
>
> 1. 创建一个空的简单 JavaScript 对象（即 `{}`）；
> 2. 链接该对象（设置该对象的 `constructor`）到另一个对象 ；
> 3. 将步骤 1 新创建的对象作为 `this` 的上下文 ；
> 4. 如果该函数没有返回对象，则返回 `this`。

## 考虑返回值

- 假如构造函数有返回值且是对象，返回这个对象
- 假如构造函数有返回值且不是对象，返回创建的空对象
- 假如构造函数没有返回值，返回创建的空对象

## 实现

```js
function myNew() {
  let obj = {}
  let Constructor = [].shift.call(arguments)
  obj.__proto__ = Constructor.prototype
  let ret = Constructor.apply(obj, arguments)
  return typeof ret === 'object' ? ret : obj
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

## 扩展：可以使用 new 一个箭头函数吗？

箭头函数没有 `prototype`、没有自己的 `this` 指向、不可以使用 `arguments`，自然不可以 `new`

## 更多资料

- [第 14 题：情人节福利题，如何实现一个 new](https://github.com/Advanced-Frontend/Daily-Interview-Question/issues/12)
- [JavaScript 深入之 new 的模拟实现](https://github.com/mqyqingfeng/Blog/issues/13)
- [JS 的 new 到底是干什么的？](https://zhuanlan.zhihu.com/p/23987456)
