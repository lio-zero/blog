# for-in 和 for-of 的区别

在对数组或对象进行遍历时，我们经常会使用到到 `for ... in` 和 `for ... of`，本文将来讲解以下这两者的区别。

## 区别

- 在 `for ... in` 和 `for ... of` 语句上迭代的值是不同的。

`for ... in` 迭代对象的可枚举属性键。而 `for ... of` 迭代对象属性的值。

```js
const list = ['a', 'b', 'c']

for (let i in list) {
  console.log(i) // '0', '1', '2'
}

for (let i of list) {
  console.log(i) // 'a', 'b', 'c'
}
```

- `for ... of` 不像 `for ... in`，它不支持迭代普通对象。

```js
const person = {
  firstName: 'Foo',
  lastName: 'Bar',
  age: 42
}

// TypeError: `person` is not iterable
for (let k of person) {
  // ...
}
```

这是因为普通对象不可迭代。为了解决这个问题，我们可以使用 `Object.keys()` 方法来迭代对象属性：

```js
for (let k of Object.keys(person)) {
  console.log(k, ':', person[k])
}

// firstName: Foo
// lastName: Bar
// age: 42
```

- `for...of` 支持迭代 Unicode 字符串。

```js
const msg = 'Hell😀 W😀rld'

// for ... in
for (let i in msg) {
  console.log(msg[i]) // 'H', 'e', 'l', 'l', '�', ' ', 'W', '�', '�', 'r', 'l', 'd'
}

// for ... of
for (let c of msg) {
  console.log(c) // 'H', 'e', 'l', 'l', '😀', ' ', 'W', '😀', 'r', 'l', 'd'
}
```

- `for ... of` 循环可以通过 `await` 关键字在每次迭代中等待异步任务完成：

```js
for await (... of ...) {
  // ...
}
```

## 建议

- 不建议向原始对象（如 `Array`、`Boolean`、`Number`、`String` 等）添加自定义方法，因为 `for ... in` 声明遍历枚举的特性，它将包括添加到原型中的新方法。

```js
Array.prototype.isEmpty = function () {
  return (this.length = 0)
}

const a = ['cat', 'dog', 'mouse']
for (let i in a) {
  console.log(i) // '0', '1', '2', 'isEmpty'
}
```

- `for ... in` 已被[弃用](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Deprecated_and_obsolete_features#Statements)。相反，使用 `for ... of`：

```js
const addressBook = new Map()
addressBook.set('Foo', '111-222-333')
addressBook.set('Bar', '444-555-666')

for (const [name, phone] of addressBook) {
  console.log(name, ':', phone)
}

// Foo: 111-222-333
// Bar: 444-555-666
```

默认情况下，数组或对象的所有属性都将出现在 `for ... in`。但是，这种行为是可以避免的。使用 `Object.defineProperty` 可以决定属性是否可枚举。

```js
let person = {
  firstName: 'Foo',
  lastName: 'Bar'
}

// age 属性不可枚举
Object.defineProperty(person, 'age', {
  value: 42,
  enumerable: false
})

for (let i in person) {
  console.log(i) // 'firstName', 'lastName'
}
```

## 更多资料

[What is the difference between ( for... in ) and ( for... of ) statements?](https://stackoverflow.com/questions/29285897/what-is-the-difference-between-for-in-and-for-of-statements)
