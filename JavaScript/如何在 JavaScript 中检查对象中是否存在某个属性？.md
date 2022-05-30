# 如何在 JavaScript 中检查对象中是否存在某个属性？

- `in` 操作符用于检查对象的自有属性和继承来的属性是否存在。

```js
let user = {
  name: 'lio'
}

console.log('age' in user) // false
console.log('name' in user) // true
console.log('constructor' in user) // true
```

- `hasOwnProperty` 方法只检查属性是否存在于当前对象中，而忽略原型链。

```js
console.log(user.hasOwnProperty('age')) // false
console.log(user.hasOwnProperty('name')) // true
console.log(user.hasOwnProperty('constructor')) // false
```

- 使用点运算符或括号符号 `user["prop"]`。如果属性存在，它将返回该属性的值，否则将返回 `undefined`

```js
console.log(user['age']) // undefined
console.log(user['name']) // "lio"
```

- ES11 的 `?.` 可选链操作符的出现是为了解决 "不存在的属性" 引起的错误问题，我们刚好可以来检查对象属性是否存在。

```js
console.log(user?.age) // undefined
console.log(user?.name) // "lio"
```
