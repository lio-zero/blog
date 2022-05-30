# 如何在 JavaScript 中判断一个对象是否为空？

我们想要判断对象是否为空，像基本类型那样比较是不可以的

```js
const obj = {}
console.log(obj === {}) // false
```

可以看到，两个都是空对象，但是进行比较，返回的是 `false`。

因为对象是引用类型，使用 `===` 或 `==` 比较的是引用（内存地址），因此您不能使用它们比较两个对象。

下面我们来介绍三种判断空对象的方法。

## for...in

> [`for...in`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...in) 语句以任意顺序遍历一个对象的除 [Symbol](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Symbol) 以外的[可枚举](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Enumerability_and_ownership_of_properties)属性。

根据 `for...in` 遍历对象，如果存在则返回 `非空`，否则返回 `空`。

```js
const obj = {}
const user = {
  name: 'IU'
}

const isEmpty = (obj) => {
  for (let i in obj) {
    return '非空'
  }
  return '空'
}

console.log(empty(obj)) // "空"
console.log(empty(user)) // "非空"
```

## JSON.stringify()

利用 JSON 的 `JSON.stringify()` 方法来判断。将空对象转化为字符串 `'{}'` 来进行判断。

```js
const isEmpty = (obj) => (JSON.stringify(obj) === '{}' ? '空' : '非空')

console.log(empty(obj)) // "空"
console.log(empty(user)) // "非空"
```

## Object.keys()

> `Object.keys()` 方法会返回一个由一个给定对象的自身可枚举属性组成的数组。如果对象为空，将返回一个空数组。

检查对象是否为空的最简单方法是检查它是否有键。我们使用 ES6 中的 `Object.keys()` 方法检查对象是否有键。

```js
const isEmpty = (obj) => (Object.keys(obj).length === 0 ? '空' : '非空')

console.log(empty(obj)) // "空"
console.log(empty(user)) // "非空"
```
