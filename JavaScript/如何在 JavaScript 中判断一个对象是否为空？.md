# 如何在 JavaScript 中判断一个对象是否为空？

我们想要判断对象是否为空，像基本类型那样比较是不可以的

```js
const obj = {}

obj === {} // false
```

可以看到，两个都是空对象，但是进行比较，返回的是 `false`。

因为对象是引用类型，使用 `===` 或 `==` 比较的是引用（内存地址），因此您不能使用它们比较两个对象。

下面我们来介绍几种判断空对象的方法。

## for...in

> [`for...in`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...in) 语句以任意顺序遍历一个对象的除 [Symbol](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Symbol) 以外的[可枚举](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Enumerability_and_ownership_of_properties)属性。

根据 `for...in` 遍历对象，如果存在则返回 `false`，否则返回 `true`。

```js
function isEmpty(obj) {
  for (var prop in obj) {
    // 判断是否自身属性
    if (Object.prototype.hasOwnProperty.call(obj, prop)) {
      return false
    }
  }

  return JSON.stringify(obj) === '{}'
}

isEmpty({}) // true
isEmpty({ name: 'O.O' }) // false
```

## JSON.stringify()

利用 JSON 的 `JSON.stringify()` 方法来判断。将空对象转化为字符串 `'{}'` 来进行判断。

```js
const isEmpty = (obj) => JSON.stringify(obj) === '{}'

isEmpty({}) // true
isEmpty({ name: 'O.O' }) // false
```

## Object.keys()

> `Object.keys()` 方法会返回一个由一个给定对象的自身可枚举属性组成的数组。如果对象为空，将返回一个空数组。

检查对象是否为空的最简单方法是检查它是否有键。我们使用 ES5 新增的 `Object.keys()` 方法检查对象是否有键。

```js
const isEmpty = (obj) => Object.keys(obj).length === 0

isEmpty({}) // true
isEmpty({ name: 'O.O' }) // false
```

> **Tips**：从 ES6 开始，`Object.keys` 传入非对象参数将被强制转换为对象。点击了解：[非对象强制](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/keys#non-object_coercion)。

## 第三方库

- Underscore.js：[`_.isEmpty()`](https://underscorejs.org/#isEmpty)
- Lodash：[`_.isEmpty()`](https://lodash.com/docs/4.17.15#isEmpty)
- Ramda：[`_.isEmpty()`](https://ramdajs.com/docs/#isEmpty)

## 更多资料

Stack Overflow 有很多这方面的讨论，包括其他方案及以上提供方法的基准测试等，详细内容请查阅 [How do I test for an empty JavaScript object?](https://stackoverflow.com/questions/679915/how-do-i-test-for-an-empty-javascript-object/32108184#32108184)。
