# 如何在 JavaScript 中将数组转为对象

首先，我们需要明白对象具有键和值。

```js
const object = {
  key: 'value'
}
```

如果我们想把某个东西转换成一个**对象**，我们需要传递具有这两个要求的东西：**键和值**。

满足这些要求的参数有两种类型：

- 具有嵌套键值对的数组
- `Map` 对象

## 数组

这是一个带有键值对的嵌套数组

```js
const nestedArray = [
  ['key 1', 'value 1'],
  ['key 2', 'value 2']
]
```

当我们应用它时，我们可以使用 `Object.fromEntries` 方法获取对象：

```js
Object.fromEntries(nestedArray) // { key 1: "value 1", key 2: "value 2"}
```

## Map 对象

ES6 为我们带来了一个名为 [`Map`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map#Objects_vs._Maps) 新的数据结构，它与 `Object` 非常相似。

> TC39：`Map` 对象是键/值对的集合，其中键和值可以是任意的 ECMAScript 语言值。

让我们创建新的 `map` 对象。

```js
// 使用 constructor
const map = new Map([
  ['key 1', 'value 1'],
  ['key 2', 'value 2']
])

// 或者我们可以使用实例方法 set
const map = new Map()
map.set('key 1', 'value 1')
map.set('key 2', 'value 2')

// Map(2) {"key 1" => "value 1", "key 2" => "value 2"}
```

现在让我们使用 `object.fromEntries` 将 `map` 转换成一个对象：

```js
Object.fromEntries(map) // { key 1: "value 1", key 2: "value 2"}
```

## 其他类型

尝试将其他数据类型传递到 `Object.fromEntries` 时要小心。以下列出的所有情况将一致抛出一个错误：

| 类型        |                                      |
| :---------- | :----------------------------------- |
| `undefined` | `Object.fromEntries(undefined)`      |
| `Null`      | `Object.fromEntries(null)`           |
| `Boolean`   | `Object.fromEntries(true)`           |
| `Number`    | `Object.fromEntries(100)`            |
| `String`    | `Object.fromEntries("hi")`           |
| `Object`    | `Object.fromEntries({key: "value"})` |
| 扁平化数组  | `Object.fromEntries([1,2,3])`        |

> **注意**：确保只传递一个键值对。

## `Object.fromEntries` 和 `Object.entries` 的区别

`Object.entries` 方法返回一个给定对象自身可枚举属性的键值对数组。

```js
const object = { key1: 'value1', key2: 'value2' }
const array = Object.entries(object) // [ ["key1", "value1"], ["key2", "value2"] ]

Object.fromEntries(array) // { key1: 'value1', key2: 'value2' }
```

### 对象到对象转换

你可以在最初的 [TC39 提案](https://github.com/tc39/proposal-object-from-entries) 找到引入 `Object.entries` 方法的缘由。

通常，当我们选择使用 `Object.entries` 时，是因为它让我们能够访问一些漂亮的数组方法。

```js
const user = {
  name: 'O.O',
  age: 18,
  address: 'xxx'
}

console.log(Object.entries(user).filter(([key, value]) => key !== 'age'))
// [["name", "O.O"], ["address", "xxx"]]
```

它返回了一组嵌套数组，而不是我们想要的对象转对象，后面又引入了 `fromEntries` 方便该操作。

```js
const arr = [
  ['name', 'O.O'],
  ['address', 'xxx']
]
console.log(Object.fromEntries(arr)) // {name: 'O.O', address: 'xxx'}

const user = {
  name: 'O.O',
  age: 18,
  address: 'xxx'
}

Object.fromEntries(Object.entries(user).filter(([key, value]) => key !== 'age'))
// {name: 'O.O', address: 'xxx'}
```

## 浏览器支持情况

除了 IE，大多数主流浏览器都支持这种方法：

![Object.fromEntries()](https://upload-images.jianshu.io/upload_images/18281896-6ca9a112aafbf0fe.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 替代方案

`Object.fromEntries` 于 2019 年推出，所以它仍然是相当新的的一个方法。因此，在照顾旧浏览器的情况下，我们需要其他的替代方案，将一个键值对数组转化为一个对象以更好的支持。

### Reduce

将数组转换为对象的一种常用方法是使用 `reduce` 方法

```js
const array = [
  ['key1', 'value1'],
  ['key2', 'value2']
]

const map = new Map([
  ['key1', 'value1'],
  ['key2', 'value2']
])

function toObject(pairs) {
  return Array.from(pairs).reduce(
    (acc, [key, value]) => Object.assign(acc, { [key]: value }),
    {}
  )
}

toObject(array) // { key1: 'value1', key2: 'value2' }
toObject(map) // { key1: 'value1', key2: 'value2' }
```

### 第三方库

一些第三方库，例如：Underscore 和 Lodash 也提供了可将键值对转换为对象的方法。

`_.object` — 将数组转换为对象。传递单个键值对列表，或键列表和值列表。

```js
// Underscore
const array = [
  ['key1', 'value1'],
  ['key2', 'value2']
]

_.object(array) // { key1: 'value1', key2: 'value2' }
```

`_.fromPairs` — 此方法返回由键值对组成的对象。

```js
// Lodash
const array = [
  ['key1', 'value1'],
  ['key2', 'value2']
]

_.fromPairs(array) // { key1: 'value1', key2: 'value2' }
```
