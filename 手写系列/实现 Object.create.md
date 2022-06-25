# 实现 Object.create

> [`Object.create()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/create) 方法创建一个新对象，使用现有的对象来提供新创建的对象的 `__proto__`。

**用法**：

```js
const user = {
  name: 'O.O',
  sayHi() {
    console.log(this.name, this.age)
  }
}

const me = Object.create(user)

me.name = 'D.O' // 继承的属性可以被覆盖
me.age = 18 // age 是设置在 me 上的属性，而不是设置在 user 上的属性

me.sayHi() // D.O 18
```

**实现思路**：定义一个空的构造函数，然后将传入的对象作为原型指向构造函数的原型对象，通过 `new` 运算符创建一个空对象。

```js
function create(obj) {
  function F() {}
  F.prototype = obj
  return new F()
}

// 同样运行执行效果。
const me = create(user)
me.name = 'D.O'
me.age = 18

me.sayHi() // D.O 18
```

**注意**：`Object.create` 只能进行浅拷贝操作

```js
const user = {
  name: 'IU',
  age: 18,
  friend: {
    name: 'Lay',
    age: 20
  }
}

const me = Object.create(user)

me.friend.name = 'D.O'

console.log(me.friend.name) // "D.O"
console.log(user.friend.name) // "D.O"
```
