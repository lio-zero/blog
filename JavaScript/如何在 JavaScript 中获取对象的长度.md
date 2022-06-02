# 如何在 JavaScript 中获取对象的长度

与数组不同，获取对象长度总是很棘手的。

有两种方法获取对象长度：

- `Object.keys` 返回对象的所有可枚举属性键的数组。
- 使用 **Lodash** 库的 `_.size` 方法

```js
const object = { id: 1, status: 0 }

// 使用 JavaScript
Object.keys(object).length // 2

// 使用 Lodash
_.size(object) // 2
```

## 为什么我们不能调用对象 `length`

你可能想知道为什么我们不能直接调用对象 `length` 属性。让我们看看当我们这样做时会发生什么：

```js
const object = { id: 1, status: 0 }

object.length // undefined

object.hasOwnProperty('length') // false
```

可以看到，它没有 `length` 属性。我们需要知道，仅 `string` 和 `array` 具有属性 `length`。

```js
const string = 'hello'
const array = [1, 2, 3]

string.hasOwnProperty('length') // true
array.hasOwnProperty('length') // true
```

## 什么是可枚举属性？

我在开头提到 `Object.keys` 返回一个**可枚举**属性键数组。所以，让我们找出 `enumerable` 属性的来源。

### 指定属性

通常，当我们要向对象添加属性时，我们可能只使用点符号：

```js
const object = {}

object.id = 1

console.log(object) // { id: 1 }
```

### 定义属性

或者，我们也可以使用 `Object.defineProperty`，该方法会直接在一个对象上定义一个新属性，或者修改一个对象的现有属性，并返回此对象。

它接受 3 个参数：

- `obj` — 要定义属性的对象。
- `prop` — 要定义或修改的属性的名称或 `Symbol` 。
- `descriptor` — 要定义或修改的属性描述符。

```js
Object.defineProperty(obj, prop, descriptor)
```

让我们用这个方法定义一个属性：

```js
const object = {}

Object.defineProperty(object, id, {
  value: 1
})

console.log(object) // {}
```

为什么是空的？为什么我们的属性没有出现 🤔？

那是因为当我们这样定义一个属性时，`enumerable` 属性的默认为 `false`。所以如果我们想让它出现，我们需要设置为 `true`

```js
const object = {}

Object.defineProperty(object, id, {
  value: 1,
  enumerable: true // 👈
})

console.log(object) // { id: 1 }
```

### 可枚举默认为 true

让我们回到用点符号设置的对象属性示例。为什么它会自动显示？好吧，那是因为当我们这样分配属性时，`enumerable` 属性会自动设置为 `true`。

```js
const object = {}

object.id = 1

object.propertyIsEnumerable(id) // true
```

### 可枚举摘要

对于我们大多数人来说，在定义属性时很少触及可枚举属性。对于我们来说，这只是一种控制在使用 `Object.keys` 遍历对象时，创建的特定属性是显示还是隐藏的方法。

如果您想了解有关可枚举性的更多信息，可以查阅 [ECMAScript 6 中的可枚举性](http://2ality.com/2015/10/enumerability-es6.html)。

> 因此，`enumerable` 属性用于隐藏不应迭代的属性。这就是在 ECMAScript 1 中引入可枚举性的原因。

## `Object.keys` 与 `Object.getOwnPropertyNames`

现在您已经了解了 `enumerable`，让我们介绍另一个方法 `Object.getOwnPropertyNames`，您可以将其视为获取长度的选项。

```js
const object = { id: 1 }

Object.defineProperty(object, status, {
  value: 0,
  enumerable: false
})

Object.keys(object) // ['id']
Object.getOwnPropertyNames(object) // ['id', 'status']
```

如您所见，`object.getOwnPropertyNames` 将返回所有属性键（即，可枚举和不可枚举属性），而 `object.keys` 只返回可枚举属性键。

如上所述，可枚举属性可能出于某种原因而被隐藏，因此您可能不想访问它。因此，`Object.getOwnPropertyName` 可能不是要用于获取对象长度的方法。

## 带 Symbols 类型的对象长度

`object.keys` 在默认情况下获取对象长度之前，我想再指出一个考虑因素。在 ES6 中，引入了一种新的数据类型，称为 `symbol`。它可以用作对象的属性名。

```js
const user = {
  [Symbol('id')]: 1,
  status: 0
}
```

但问题是当你有一个 `symbol` 属性名时。`Object.keys` 和 `Object.getOwnPropertyNames` 都不起作用。

```js
Object.keys(user) // ["status"] <-- no symbol

Object.getOwnPropertyNames(user) // ["status"] <-- no symbol
```

因此，解决方案是使用 `Object.getOwnPropertySymbols`。

```js
Object.getOwnPropertySymbols(user) // [Symbol(id)]
```

现在结合这两种方法，您将得到适当的长度。

```js
const enumerableLength = Object.keys(user).length
const symbolLength = Object.getOwnPropertySymbols(user).length

const totalObjectLength = enumerableLength + symbolLength // 2
```
