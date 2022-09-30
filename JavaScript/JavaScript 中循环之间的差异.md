# JavaScript 中循环之间的差异

在 JavaScript 中使用循环时，需要正确定义两个关键内容：可枚举属性（`enumerable properties`）和可迭代对象（`iterable objects`）。

## 可枚举的属性

可枚举对象的一个定义特征是，当我们通过赋值运算符将属性赋值给对象时，我们将内部可枚举标志（`enumerable`）设置为 `true`。这是默认值。

但是，我们可以通过将其设置为 `false` 来更改此行为。

经验法则是，可枚举属性总是出现在 `for...in` 循环中。

让我们看看这一点：

```js
const users = {}
users.languages = 'JavaScript'

Object.getOwnPropertyDescriptor(users, 'languages')
// output -> { value: 'JavaScript', writable: true, enumerable: true, configurable: true }

// 在循环中对我们使用的属性进行更多的控制
Object.defineProperty(users, 'role', {
  value: 'Admin',
  writable: true,
  enumerable: false
})
// {languages: 'JavaScript', role: 'Admin'}

for (const item in users) {
  console.log(item) // languages
}
```

可以看到，我们为 `users` 变量添加了一个 `languages` 属性，使用 `Object.getOwnPropertyDescriptor` 方法输出 `languages` 属性描述符的 `enumerable` 属性为 `true`。

使用 `Object.defineProperty` 为添加 `role` 属性，并将 `enumerable` 设置为 `false`，在 `for...in` 循环中并没有输出 `role` 属性。即 `for...in` 循环中的属性为可枚举属性。

### 可迭代对象

如果一个对象定义了它的迭代行为，那么它是可迭代的。在本例中，将在 `for...of` 构造中循环的值将定义其迭代行为。可迭代的内置类型包括 `Array`、`String`、`Set` 和 `Map`，而 `Object` 不可迭代，因为它没有指定 `@iterator` 方法。

> 推荐：阮一峰老师的 [Iterator 和 for...of 循环](https://es6.ruanyifeng.com/#docs/iterator)，也可以看看冴羽的 [ES6 系列之迭代器与 for of](https://github.com/mqyqingfeng/Blog/issues/90)。

基本上，在 JavaScript 中，**所有可迭代对象都是可枚举对象，但并非所有可枚举对象都是可迭代对象**。

这里有一种概念化的方法：`for...in` 查找数据中的对象，而 `for...of` 查找重复序列。

让我们看看这一切在与 `Array` 数据类型一起使用时的效果：

```js
const languages = ['JavaScript', 'Python', 'Go']

// 与 for...in 循环一起使用
for (const language in languages) {
  console.log(language)
}
// output：
// 0
// 1
// 2

// 与 for...of 循环一起使用
for (const language of languages) {
  console.log(author)
}
// output：
// JavaScript
// Python
// Go
```

在使用这种构造时，需要牢记的一点是，如果调用了 `typeof`，并且输出为 `object`，那么您可以使用 `for...in` 循环。

让我们看看这个对 `languages` 变量的操作：

```js
typeof languages // "object" -> 因此我们可以在中使用 for
```

乍一看，这可能令人惊讶，但需要注意的是，数组是一种特殊类型的对象，以索引为键。知道 `for...in` 将在构造中查找对象可以极大地帮助我们。当 `for...in` 循环找到一个对象时，它将在每个键上循环。

我们可以将 `for...in` 循环在 `languages` 数组上的方式可视化如下：

```js
const languages = {
  0: 'JavaScript',
  1: 'Python',
  2: 'Go'
}
```

**注意**：如果它可以被追踪到一个对象（或者从对象原型链继承它），`for...in` 将以没有特定顺序遍历键。

同时，如果它实现了一个迭代器 `for...of` 构造，它将在每次迭代中循环遍历该值。

## `forEach` 和 `map` 方法

虽然 `forEach` 和 `map` 方法可以用来实现相同的目标，但它们的行为和性能特性有所不同。

当函数被调用时，它们都会接收一个回调作为参数。

考虑以下片段：

```js
const scoresEach = [2, 4, 8, 16, 32]
const scoresMap = [2, 4, 8, 16, 32]
const square = (num) => num * num
```

让我们详细介绍一下它们在操作上的一些差异。

`forEach` 返回 `undefined`，而 `map` 返回一个新的 `array`：

```js
let newScores = []
const resultWithEach = scoresEach.forEach((score) => {
  const newScore = square(score)
  newScores.push(newScore)
})

const resultWithMap = scoresMap.map(square)

console.log(resultWithEach) // undefined
console.log(resultWithMap) // [4, 16, 64, 256, 1024]
```

`Map` 是一个纯函数，同时 `forEach` 执行一些突变：

```js
console.log(newScores) // [4, 16, 64, 256, 1024]
```

在我看来，`map` 支持函数式编程范式。我们不必总是执行突变来获得期望的结果，不像 `forEach`，我们必须突变 `newScores` 变量。在每次运行时，当提供相同的输入时，`map` 函数将产生相同的结果。同时，`forEach` 对应项将从上一个突变的先前值中提取。

### 链式调用

使用 `map` 可以进行链式调用，因为返回的结果是一个数组。因此，可以对结果立即调用任何其他数组方法。换句话说，我们可以调用 `filter`、`reduce`、`some` 等方法。这在 `forEach` 中是不可能的，因为返回的值为 `undefined` 的。

### 性能

`map` 方法的性能往往优于 `forEach` 方法。

检查使用 `map` 和 `forEach` 实现的等效代码块的性能。平均而言，您将看到 `map` 函数的执行速度至少快 50%。

## 结论

在上面讨论的所有循环结构中，给我们最多控制权的是 `for...of` 循环。我们可以将其与关键字 `return`、`continue` 和 `break` 一起使用。这意味着我们可以指定对数组中的每个元素要发生什么，以及是否要提前离开或跳过。

## 更多资料

- [Array Iteration](https://gist.github.com/ljharb/58faf1cfcb4e6808f74aae4ef7944cff)
- [JavaScript 数组方法总结](https://github.com/lio-zero/blog/blob/main/JavaScript/JavaScript%20%E6%95%B0%E7%BB%84%E6%96%B9%E6%B3%95%E6%80%BB%E7%BB%93.md)
