# JavaScript Lodash 工具库

## 简介

[Lodash](https://lodash.com/) 提供模块化、性能和额外功能的现代 JavaScript 实用工具库。它通过降低 Array、Number、Objects、String 等等的使用难度从而让 JavaScript 变得更简单，提高开发者效率。类似的还有 [Underscore](https://www.underscorejs.com.cn/) 和 [Lazy](http://danieltao.com/lazy.js/)，还有一个 [Ramda](https://ramdajs.com/) 库，它是用柯里化实现的。

> **支持情况**：Chrome 74-75，Firefox 66-67，IE 11，Edge 18，Safari 11-12 & Node.js 8-12

## 安装

- [核心功能](https://raw.githubusercontent.com/lodash/lodash/4.17.15-npm/core.js)、[（~4kB）](https://raw.githubusercontent.com/lodash/lodash/4.17.15-npm/core.min.js)
- [完整功能](https://raw.githubusercontent.com/lodash/lodash/4.17.15-npm/lodash.js)、[（~24kB](https://raw.githubusercontent.com/lodash/lodash/4.17.15-npm/lodash.min.js))

CDN：

```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/lodash.js/4.17.21/lodash.min.js"></script>
```

NPM：

```bash
npm i lodash
```

以下将介绍 Loadsh 的常用方法

## Find

`_.filter` 返回一个新的过滤后的数组。

```js
var users = [
  { user: 'barney', age: 36, active: true },
  { user: 'fred', age: 40, active: false }
]

_.filter(users, (o) => !o.active) // { 'user': 'fred',   'age': 40, 'active': false }
_.filter(users, { age: 36, active: true }) // { 'user': 'barney', 'age': 36, 'active': true },
_.filter(users, ['active', false]) // { 'user': 'fred', 'age': 40, 'active': false }
_.filter(users, 'active') // { 'user': 'barney',  'age': 36, 'active': true }
```

`_.find` 和 `_.findLast` 返回匹配元素，否则返回 `undefined`。

```js
var users = [
  { user: 'barney', age: 36, active: true },
  { user: 'fred', age: 40, active: false },
  { user: 'pebbles', age: 1, active: true }
]

_.find(users, (o) => o.age < 40) // { 'user': 'barney',  'age': 36, 'active': true }
_.find(users, { age: 1, active: true }) // { 'user': 'pebbles', 'age': 1,  'active': true }
_.find(users, ['active', false]) // { 'user': 'fred', 'age': 40, 'active': false }
_.find(users, 'active') // { 'user': 'barney',  'age': 36, 'active': true }

_.findLast([1, 2, 3, 4], (n) => n % 2 == 1) // 3
```

它们都适用于数组和对象。

## set/get

### set

`_.set` 返回 `object`。设置 `object` 对象中对应 `path` 属性路径上的值，如果`path`不存在，则创建。 缺少的索引属性会创建为数组，而缺少的属性会创建为对象。 使用[`_.setWith`](https://www.lodashjs.com/docs/lodash.set#setWith) 定制`path`创建。

```js
let object = { a: [{ b: { c: 3 } }] }

_.set(object, 'a[0].b.c', 4)
console.log(object.a[0].b.c) // 4

_.set(object, ['x', '0', 'y', 'z'], 5)
console.log(object.x[0].y.z) // 5
```

### get

`_.get` 返回解析的值。根据 `object` 对象的 `path` 路径获取值。 如果解析 value 是 `undefined` 会以 `defaultValue` 取代。

```js
let object = { a: [{ b: { c: 3 } }] }

_.get(object, 'a[0].b.c') // 3
_.get(object, ['a', '0', 'b', 'c']) // 3
_.get(object, 'a.b.c', 'default') // 'default'
```

## 迭代

`_.forEach` 和 `_.forEachRight` 返回集合 `collection`。

```js
_([1, 2]).forEach(function (value) {
  console.log(value) // 1 2
})

_.forEach({ a: 1, b: 2 }, (value, key) => {
  console.log(key) // a b (不保证迭代顺序)
})

_.forEachRight([1, 2], function (value) {
  console.log(value) // 2 1.
})
```

`_.map` 返回新的映射后数组。

```js
function square(n) {
  return n * n
}

_.map([4, 8], square) // [16, 64]
_.map({ a: 4, b: 8 }, square) // [16, 64] (不保证迭代顺序)

var users = [{ user: 'barney' }, { user: 'fred' }]

_.map(users, 'user') // ['barney', 'fred']
```

`_.every` 如果所有元素经 predicate（断言函数） 检查后都都返回真值，那么就返回`true`，否则返回 `false`。

```js
_.every([true, 1, null, 'yes'], Boolean) // false

let users = [
  { user: 'barney', age: 36, active: false },
  { user: 'fred', age: 40, active: false }
]

_.every(users, { user: 'barney', active: false }) // false
_.every(users, ['active', false]) // true
_.every(users, 'active') // false
```

## Array

`_.chunk` 返回一个包含拆分区块的新数组（注：相当于一个二维数组）。

```js
_.chunk(['a', 'b', 'c', 'd'], 2) // [['a', 'b'], ['c', 'd']]
```

`_.compact` 返回过滤掉假值的新数组。

```js
_.compact([0, 1, false, 2, '', 3]) // [1, 2, 3]
```

`_.fill` 返回 `array`。使用 `value` 值来填充（替换） `array`，从`start`位置开始, 到`end`位置结束（但不包含`end`位置）。

```js
_.fill([1, 2, 3], 'a') // ['a', 'a', 'a']
_.fill(Array(4), 'x') // [ 'x', 'x', 'x', 'x' ]
_.fill([4, 6, 8, 10], '*', 1, 3) // [4, '*', '*', 10]

_.flattenDeep([1, [2, [3, [4]], 5]]) // [1, 2, 3, 4, 5]
```

`_.flatten` 返回减少嵌套层级后的新数组。

```js
_.flatten([1, [2, [3, [4]], 5]]) // [1, 2, [3, [4]], 5]
```

`_.flattenDeep` 返回一个的新一维数组。

```js
_.flattenDeep([1, [2, [3, [4]], 5]]) // [1, 2, 3, 4, 5]
```

### 过滤

`_.drop` 返回 `array` 剩余切片。

```js
_.drop([1, 2, 3]) // [2, 3]
_.drop([1, 2, 3], 2) // [3]
_.drop([1, 2, 3], 5) // []
_.drop([1, 2, 3], 0) // [1, 2, 3]

_.dropRight([1, 2, 3]) // [1, 2]
```

- `_.take` 返回 `array` 数组的切片（从起始元素开始`n`个元素）。
- `_.takeRight` 返回 `array` 数组的切片（从结尾元素开始`n`个元素）。

```js
_.take([1, 2, 3]) // [1]
_.take([1, 2, 3], 2) // [1, 2]
_.take([1, 2, 3], 5) // [1, 2, 3]
_.take([1, 2, 3], 0) // []

_.takeRight([1, 2, 3], 2) // [2, 3]
```

`_.slice` 返回 数组 `array` 裁剪部分的新数组。

```js
_.slice([1, 2, 3], 0, 2) // [1, 2]
```

`_.initial` 返回截取后的数组`array`

```js
_.initial([1, 2, 3]) // [1, 2]
```

`_.rest` 返回新的函数。

```js
let say = _.rest(function (what, names) {
  return (
    what +
    ' ' +
    _.initial(names).join(', ') +
    (_.size(names) > 1 ? ', & ' : '') +
    _.last(names)
  )
})

say('hello', 'fred', 'barney', 'pebbles') // 'hello fred, barney, & pebbles'
```

`_.dropWhile` 返回 `array` 剩余切片。

```js
_.dropWhile(list, 'active')            // 像过滤器一样工作
_.dropWhile(list, 'active', true)
_.dropWhile(list, { active: true })
_.dropWhile(list, (n) => ...)
_.dropRightWhile(list, ...)

let users = [
  { 'user': 'barney',  'active': false },
  { 'user': 'fred',    'active': false },
  { 'user': 'pebbles', 'active': true }
]

_.dropWhile(users, (o) => !o.active) // { 'user': 'pebbles', 'active': true }
_.dropWhile(users, { 'user': 'barney', 'active': false }) // [{ 'user': 'barney',  'active': false }, {...}]
_.dropWhile(users, ['active', false]) // { 'user': 'pebbles', 'active': true }
_.dropWhile(users, 'active') // [{user: "barney", active: false}, {...}, {...}]
```

`_.without` 返回过滤值后的新数组。

```js
_.without([2, 1, 2, 3], 1, 2) // [3]
```

`_.remove` 返回移除元素组成的新数组。

```js
let array = [1, 2, 3, 4]
let evens = _.remove(array, (n) => {
  return n % 2 == 0
})

console.log(array) // [1, 3]
console.log(evens) // [2, 4]
```

### 访问

`_.first` 返回数组 `array` 的第一个元素。

```js
_.head([1, 2, 3]) // 1
_.head([]) // undefined
```

`_.last` 返回 `array` 中的最后一个元素

```js
_.last([1, 2, 3]) // 3
```

`_.at` 返回选中值的数组。

```js
_.at([1, 2, 3], 0) // [1]
_.at([1, 2, 3], [0, 1]) // [1, 2]

let object = { a: [{ b: { c: 3 } }, 4] }

_.at(object, ['a[0].b.c', 'a[1]']) // [3, 4]
```

### Sets

`_.uniq` 返回新的去重后的数组。

```js
_.uniq([2, 1, 2]) // [2, 1]
```

`_.difference` 返回一个过滤值后的新数组。

```js
_.difference([3, 2, 1], [4, 2]) // [3, 1]
```

`_.intersection` 返回一个包含所有传入数组交集元素的新数组。

```js
_.intersection([2, 1], [4, 2], [1, 2]) // [2]
```

`_.union` 返回一个新的联合数组。

```js
_.union([2], [1, 2]) // [2, 1]
```

`_.concat` 返回连接后的新数组。

```js
let array = [1]
let other = _.concat(array, 2, [3], [[4]])

console.log(other) // [1, 2, 3, [4]]
console.log(array) // [1]
```

### Index

`_.findIndex` 和 `_.findLastIndex` 返回找到元素的索引值（index），否则返回 `-1`。

```js
let users = [
  { user: 'barney', active: false },
  { user: 'fred', active: false },
  { user: 'pebbles', active: true }
]

_.findIndex(list, fn)
_.findLastIndex(list, fn)

_.findIndex(users, (o) => o.user == 'barney') // 0
_.findIndex(users, { user: 'fred', active: false }) // 1
_.findIndex(users, ['active', false]) // 0
_.findIndex(users, 'active') // 2

_.findLastIndex(users, (o) => o.user == 'pebbles') // 2
```

`_.sortedIndex` 和 `_.sortedLastIndex` 返回 `value` 值应该在数组 `array` 中插入的索引位置 index。

```js
_.sortedIndex([30, 50], 40) // 1
_.sortedLastIndex([4, 5, 5, 5, 6], 5) // 4
```

`_.indexOf` 返回值 `value` 在数组中的索引位置，没有找到为返回`-1`。

```js
_.indexOf([1, 2, 1, 2], 2) // 1
_.indexOf([1, 2, 1, 2], 2, 2) // 3
```

## 柯里化（Currying）

返回预设参数的函数。

```js
let greet = (greeting, name) => `${greeting}, ${name}!`

let fn = _.partial(fn, 'hi')
fn('joe') // 'hi, joe!'
```

返回新的柯里化（curry）函数。

```js
_.curry(greet)('hi') // function(name)
_.curryRight(greet)('joe') // function(greet)
```

## 装饰器（Decorator）

### 节流和防抖

- `_.throttle` 返回节流的函数。
- `_.debounce` 返回防抖的函数。

```js
_.throttle(fn)
_.debounce(fn)
```

### 限制

```js
document.addEventListener('click', _.before(5, fn)) // 只能调用5次
document.addEventListener('click', _.after(5, fn)) // 5次后才能调用

const initialize = _.once(fn)
initialize()
initialize() // 类似 _.before(1, fn)，只调用一次 fn
```

### 其他

`_.wrap` 返回新的函数。

```js
_.wrap(_.escape, (name) => `hi ${name}`) // 与执行 name = _.escape(name) 相同
```

`_.delay` 返回计时器 id 延迟。延迟 `wait` 毫秒后调用 `fn`。 调用时，任何附加的参数会传给 `fn`。

```js
_.delay(
  (text) => {
    console.log(text)
  },
  2000,
  'Hello World!'
)
```

`_.filter` 返回一个新的过滤后的数组。

```js
function isEven(n) {
  return n % 2 == 0
}

_.filter([1, 2, 3, 4, 5, 6], _.negate(isEven)) // => [1, 3, 5]
```

`_.memoize` 返回缓存化后的函数。

```js
_.memoize(fn)
_.memoize(fn, ...)

var object = { 'a': 1, 'b': 2 };
var other = { 'c': 3, 'd': 4 };

var values = _.memoize(_.values)
values(object) // => [1, 2]

values(other) // => [3, 4]

object.a = 2
values(object) // => [1, 2]

// 修改结果缓存。
values.cache.set(object, ['a', 'b'])
values(object) // => ['a', 'b']

// 替换 _.memoize.Cache。
_.memoize.Cache = WeakMap
```

## 字符串

### 大写

转换字符串 `string` 首字母为大写，剩下为小写。

```js
_.capitalize('hello world') // 'Hello world'
```

转换 `string` 字符串为 [start case](https://en.wikipedia.org/wiki/Letter_case#Stylistic_or_specialised_usage)。

```js
_.startCase('hello_world') // 'Hello World'
```

转换字符串 `string` 为 [snake case](https://en.wikipedia.org/wiki/Snake_case)。

```js
_.snakeCase('hello world') // 'hello_world'
```

转换字符串 `string` 为 [kebab case](https://en.wikipedia.org/wiki/Letter_case#Special_case_styles)。

```js
_.kebabCase('hello world') // 'hello-world'
```

转换字符串 `string` 为 [驼峰写法](https://en.wikipedia.org/wiki/CamelCase)。

```js
_.camelCase('hello world') // 'helloWorld'
```

### 填充

`_.pad`、`_.padStart`、`_.padEnd` 返回填充后的字符串。

```js
_.pad('abc', 3) // 'abc'
_.pad('abc', 8) // '   abc  '
_.pad('abc', 8, '_-') // '_-abc_-_'

_.padStart('abc', 3) // 'abc'
_.padStart('abc', 6) // '   abc'
_.padStart('abc', 6, '_-') // '_-_abc'

_.padEnd('abc', 3) // 'abc'
_.padEnd('abc', 6) // 'abc   '
_.padEnd('abc', 6, '_-') // 'abc_-_'
```

### 去空

从 `string` 字符串中移除前面和后面的空格或指定的字符。

```js
_.trim('  str  ') // 'str'
```

从 `string` 字符串中移除后面的空格或指定的字符。

```js
_.trimStart('  str  ') // 'str  '
```

从 `string` 字符串中移除后面的空格或指定的字符。

```js
_.trimEnd('  str  ') // '  str'
_.trimEnd('-_-abc-_-', '-_-') // '-_-abc'
```

### 其他

`_.repeat` 重复 N 次给定字符串。

```js
_.repeat('-', 2) // '--'
_.repeat('abc', 2) // 'abcabc'
_.repeat('abc', 0) // ''
```

转换字符串 `string` 中[拉丁语-1 补充字母](<https://en.wikipedia.org/wiki/Latin-1_Supplement_(Unicode_block)#Character_table>)和[拉丁语扩展字母-A](https://en.wikipedia.org/wiki/Latin_Extended-A)为基本的拉丁字母，并且去除组合变音标记。

```js
_.deburr('déjà vu') // 'deja vu'
```

截断 `string` 字符串，如果字符串超出了限定的最大值。 被截断的字符串后面会以 omission 代替，omission 默认是 "..."。

```js
_.truncate('hello world', 5) // 'hello...'
_.truncate('hi-diddly-ho there, neighborino', {
  length: 24,
  separator: ' '
}) // 'hi-diddly-ho there,...'

_.truncate('hi-diddly-ho there, neighborino', {
  length: 24,
  separator: /,? +/
}) // 'hi-diddly-ho there...'

_.truncate('hi-diddly-ho there, neighborino', {
  omission: ' [...]'
}) // 'hi-diddly-ho there, neig [...]'
```

检查字符串 `string` 是否以 `target` 开头。

```js
_.startsWith('abc', 'a') // true
_.startsWith('abc', 'b') // false
_.startsWith('abc', 'b', 1) // true
```

检查字符串 `string` 是否以给定的 `target` 字符串结尾。

```js
_.endsWith('abc', 'c') // true
_.endsWith('abc', 'b') // false
_.endsWith('abc', 'b', 2) // true
```

## 对象

### 键值

- `_.keys` 返回包含属性名的数组。创建一个 object 的自身可枚举属性名为数组。
- `_.values` 返回对象属性的值的数组。创建 object 自身可枚举属性的值为数组。

```js
_.keys(obj)
_.values(obj)

function Foo() {
  this.a = 1
  this.b = 2
}

Foo.prototype.c = 3

_.keys(new Foo()) // ['a', 'b'] (无法保证遍历的顺序)
_.keys('hi') // ['0', '1']

_.values(new Foo()) // [1, 2] (无法保证遍历的顺序)
_.values('hi') // ['h', 'i']
```

## 链式调用

Loadsh 跟 jQuery 一样支持链式调用

```js
_([1, 2, 3])
  .map((n) => n * n)
  .tap(console.log)
  .thru((n) => n.reverse())
  .value()
```

支持链式调用的方法

![支持链式调用的方法](https://upload-images.jianshu.io/upload_images/18281896-9a3ff15980bea929.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

不支持链式调用的方法

![不支持链式调用的方法](https://upload-images.jianshu.io/upload_images/18281896-d1cf58a848d80739.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

---

> 关于 Ramda 的教程可以查看阮一峰老师的 [Ramda 函数库参考教程](http://www.ruanyifeng.com/blog/2017/03/ramda.html)。
>
> NPM：`npm install ramda`
> CDN：
>
> ```html
> <script src="https://cdn.jsdelivr.net/npm/ramda@latest/dist/ramda.min.js"></script>
> ```
