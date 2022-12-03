# Object.keys/values/entries

JavaScript 中的数据结构 `Set`、`Map`、`Array` 都有 `keys()`、`values()` 和 `entries()`。

普通对象也支持类似的方法，但是语法上有一些不同。

## Object.keys()

[`Object.keys()`](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Object/keys) 返回一个包含该对象所有的键的数组。

```js
let user = {
  name: 'IU',
  age: 18
}

const keyList = Object.keys(user)
console.log(keyList) // ['name', 'age']
```

### 检查数组中是否存在键

使用 `Array.prototype.includes()` 方法检查数组中是否存在键。

```js
const val = 'age'

keyList.includes(val) ? '存在 key' : '不存在 key'
```

### 计算属性数量

```js
const count = (obj) => Object.keys(obj).length

count(user) // 2
```

### 判断对象是否为空

```js
const isEmpty = (obj) => Object.keys(obj).length === 0

isEmpty({}) // true
isEmpty(user) // false
```

## Object.values()

[`Object.values(obj)`](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Object/values) 返回一个包含该对象所有的值的数组。

```js
let user = {
  name: 'IU',
  age: 18
}

// 遍历所有的值
for (let value of Object.values(user)) {
  console.log(value)
}

// IU
// 18
```

### 属性求和

```js
let price = {
  HTML: 99,
  JavaScript: 299,
  CSS: 199
}
```

- **方式一**：使用 `Object.values` 和 `for...of` 循环返回所有数的总和。

```js
const sumPrice = (val) => {
  let sum = 0
  for (let salary of Object.values(val)) {
    sum += salary
  }
  return sum
}

sumPrice(price) // 650
```

- **方式二**：使用 `Object.values` 和 `reduce` 来求和。

```js
const sumPrice = (val) => Object.values(val).reduce((a, b) => a + b, 0)

sumPrice(price) // 650
```

## Object.entries()

[`Object.entries(obj)`](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Object/entries) 返回一个包含该对象所有 `[key, value]` 键值对的数组。

```js
let user = {
  name: 'IU',
  age: 18
}

for (const [key, value] of Object.entries(user)) {
  console.log(`${key}: ${value}`)
}

// name: IU
// age: 18
```

## 你应该知道

- `Object.keys/values/entries` 方法都是用 `Object.` 来调用。
- 就像 `for...in` 循环一样，`Object.keys/values/entries` 方法会忽略使用 `Symbol` 作为键的属性。
- 如果我们想要 `Symbol` 类型的键，可以使用 [`Object.getOwnPropertySymbols()`](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertySymbols) 方法，它会返回一个只包含 `Symbol` 类型键的数组。还有另一种由 [Reflect](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Reflect) 对象提供的方法 [`Reflect.ownKeys(obj)`](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Reflect/ownKeys)，它会返回所有键。
