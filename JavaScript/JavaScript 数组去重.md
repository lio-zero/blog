# JavaScript 数组去重

## Set

> `Set` 对象是一种新数据结构，允许你存储任何类型的唯一值，无论是原始值或者是对象引用。

我们可以从给定数组创建一个新的 `Set()`，以丢弃重复的值。然后使用扩展运算符（`...`）将其转换回数组。

```js
const arr = [1, 2, 2, '1', null, '', undefined, NaN, NaN, true, false]

const unique = (arr) => [...new Set(arr)]
unique(arr) // [1, 2, "1", null, "", undefined, NaN, true, false]
```

**注意**，如果数值中存的是对象，值相同，但他们引用的是不同的内存地址，他会认为每个对象都是唯一的：

```js
[...new Set([{ a: 1 }, { a: 1 }])] // [{a: 1}, {a: 1}]

// 你可以改成如下
const obj = { a: 1 }
[...new Set([obj, obj])] // [{ a: 1 }]
```

Set 除了和扩展运算符配合，还可以和 `Array.form()` 一起使用：

```js
const unique = (arr) => Array.from(new Set(arr))

unique(arr) // [1, 2, "1", null, "", undefined, NaN, true, false]
```

## Map

创建一个空 `Map` 数据结构，使用 `Array.prototype.filter()` 和 创建一个只包含唯一值的新数组。

```js
const unique = (arr) => {
  const seen = new Map()
  return arr.filter((item) => !seen.has(item) && seen.set(item, 1))
}

unique(arr) // [1, 2, "1", null, "", undefined, NaN, true, false]
```

## Array.prototype.filter()

使用 `Array.prototype.filter()` 创建一个只包含唯一值的新数组。

```js
const unique = (arr) => arr.filter((item, index) => arr.indexOf(item) === index)

unique(arr) // [1, 2, "1", null, "", undefined, true, false]
```

**注意**：该方法直接把 `NaN` 全部过滤了。

## Array.prototype.includes()

```js
const unique = (arr) => {
  const newArr = []
  arr.map((item) => !newArr.includes(item) && newArr.push(item))
  return newArr
}

unique(arr) // [1, 2, "1", null, "", undefined, NaN, true, false]
```

也可以使用 `Array.prototype.indexOf()` 进行判断，但是存在着一个问题，该方法无法判断数组中是否含有 `NaN`。

```js
const unique = (arr) => {
  const newArr = []
  arr.map((item) => newArr.indexOf(item) == -1 && newArr.push(item))
  return newArr
}

unique(arr) // [1, 2, "1", null, "", undefined, NaN, NaN, true, false]
```

## Array.prototype.reduce()

`Array.prototype.reduce()` 方法对数组中的每个元素执行一个由您提供的 **reducer** 函数（升序执行），将其结果组合成数组返回。

```js
const unique = (arr) =>
  arr.reduce(
    (unique, item) => (unique.includes(item) ? unique : [...unique, item]),
    []
  )

unique(arr) // [1, 2, "1", null, "", undefined, NaN, true, false]
```

## Object.hasOwnProperty()

利用 `Object.hasOwnProperty` 判断是否存在对象属性。

```js
const unique = (arr) => {
  const obj = {}
  return arr.filter((item) =>
    obj.hasOwnProperty(item) ? false : (obj[item] = true)
  )
}

unique(arr) // [1, 2, "1", null, "", undefined, NaN, true, false]
```

## Object 键值对

利用**对象键值对**不能相同名来去重。

```js
const unique = (arr) => {
  const obj = {}
  // [1, 2, 3, 4, 4, 4]
  arr.forEach((value) => obj[value] = '')
  return Object.keys(obj)
}

unique(arr) // ["1", "2", "null", "", "undefined", "NaN", "true", "false"]
```

**注意两个问题**：

- 数组中的任何类型都会转为字符串类型。
- 当数组中存在相同值不同类型的数据时，会去掉其中一个。如数值 1 和字符串 '1'，会去掉其中一个。

当然，为了快速开发，我们会使用一些工具库来对数组去重。例如：

- lodash 的 `_.uniq(array)`
- Ramda 的 `R.uniq(array)`

## 更多资料

- [Remove duplicate values from JS array [duplicate]](https://stackoverflow.com/questions/9229645/remove-duplicate-values-from-js-array)
- [JavaScript 专题之数组去重](https://github.com/mqyqingfeng/Blog/issues/27)
