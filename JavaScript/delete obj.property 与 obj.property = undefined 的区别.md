# delete obj.property 与 obj.property = undefined 的区别

假设 `obj` 是一个对象，并且 `property` 是其属性的名称。

## 区别

调用 `obj.property = undefined` 将属性的值设置为 `undefined`。如果我们迭代对象的属性，该属性仍然存在并出现。

```js
let person = { name: 'John' }
person.name = undefined

person.hasOwnProperty('name') // true
name in person // true
Object.keys(person) // ['name']
for (let p in person) {
  console.log(p) // 'name'
}
```

`delete obj.property` 将从对象中删除该属性。让我们重新审视上面的示例代码，现在使用 `delete person.name`：

```js
let person = { name: 'John' }
delete person.name

person.hasOwnProperty('name') // false
name in person // false
Object.keys(person) // []

// 控制台中没有显示任何内容
for (let p in person) {
  console.log(p)
}
```

`delete` 不能删除继承的属性。

```js
const car = { branch: 'Audi' }

const a4 = Object.create(car)
console.log(a4.branch) // 'Audi'

delete a4.branch
console.log(a4.branch) // 'Audi'
```

在这种情况下，我们必须将属性设置为 `undefined`：

```js
a4.branch = undefined
console.log(a4.branch) // undefined
```

## 提示

`delete` 不适用于数组：

```js
const array = [1, 2, 3, 4, 5]

delete array[1]
console.log(array) // [1, empty, 3, 4, 5]
```

如果要从数组中删除项目，请使用 `splice` 方法。

```js
const array = [1, 2, 3, 4, 5]
array.splice(2, 1)
console.log(array) // [1, 2, 4, 5]
```

`pop` 方法可以从数组中删除最后一个元素：

```js
const array = [1, 2, 3, 4, 5]
array.pop()
console.log(array) // [1, 2, 3, 4]
```

我们可以使用 ES6 扩展运算符从对象中删除属性：

```js
const { name, ...rest } = { name: 'Foo', age: 20 }

console.log(name) // 'Foo'
console.log(rest) // { age: '20' }
```

也可以删除动态属性：

```js
const property = 'name'
const { [property]: value, ...rest } = { name: 'Foo', age: 20 }

console.log(value) // 'Foo'
console.log(rest) // { age: '20' }
```
