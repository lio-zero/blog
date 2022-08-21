# JavaScript 数组方法总结

## Array.prototype.pop()

[`pop()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/pop) 方法从数组中删除最后一个元素并返回该元素。此方法更改数组的长度。

```js
const plants = ['broccoli', 'cauliflower', 'cabbage', 'kale', 'tomato']

plants.pop() // "tomato"
console.log(plants) // ["broccoli", "cauliflower", "cabbage", "kale"]
plants.pop()
console.log(plants) // ["broccoli", "cauliflower", "cabbage"]
```

对空数组使用 `pop` 方法，不会报错，而是返回`undefined`。

```js
;[].pop() // undefined
```

> **注意**：该方法会改变原数组。

## Array.prototype.push()

[`push()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/push) 方法将一个或多个元素添加到数组的末尾，并返回数组的新长度。

```js
const animals = ['pigs', 'goats', 'sheep']

animals.push('cows') //  4
console.log(animals) // ["pigs", "goats", "sheep", "cows"]
animals.push('chickens') // 5
console.log(animals) //  ["pigs", "goats", "sheep", "cows", "chickens"]
```

> **注意**：该方法会改变原数组。

## Array.prototype.shift()

[`shift()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/shift) 方法从数组中删除第一个元素并返回该元素。此方法更改数组的长度。

```js
const arr = [1, 2, 3]
const firstElement = arr.shift()

console.log(arr) //  [2, 3]
console.log(firstElement) // 1
```

遍历并清空一个数组

```js
const arr = [1, 2, 3]
let item

while ((item = list.shift())) {
  console.log(item)
}

list // []
```

上面代码通过 `list.shift()` 方法每次取出一个元素，从而遍历数组。它的前提是数组元素不能是 `0` 或任何布尔值等于 `false` 的元素，因此这样的遍历不是很可靠。

> **注意**：该方法会改变原数组。

## Array.prototype.unshift()

[`unshift()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/unshift) 方法将一个或多个元素添加到数组的开头，并返回数组的新长度。

```js
const arr = [1, 2, 3]

console.log(arr.unshift(4, 5)) //  5
console.log(arr) // [4, 5, 1, 2, 3]
```

> **注意**：该方法会改变原数组。

## Array.prototype.join()

[`join()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/join) 方法以指定参数作为分隔符，将数组的所有元素连接为一个字符串返回。如果不提供参数，默认用逗号分隔。

```js
const arr = ['Fire', 'Air', 'Water']

console.log(arr.join()) // "Fire,Air,Water"
console.log(arr.join(' ')) // "Fire Air Water"
console.log(arr.join('-')) // "Fire-Air-Water"
```

如果数组成员是 `undefined` 或 `null` 或空位，会被转成空字符串。

```js
[undefined, null].join('#') // '#'

['a',, 'b'].join('-') // 'a--b'
```

通过 `call` 方法，`join` 方法也可以用于字符串或类数组的对象。

```js
Array.prototype.join.call('hello', '-') // "h-e-l-l-o"

const obj = { 0: 'a', 1: 'b', length: 2 }
Array.prototype.join.call(obj, '-') // 'a-b'
```

**示例**：重复字符串

使用 `join` 方法来重复字符串。

```js
Array(4).join('LOVE') // 'LOVELOVELOVE'
```

## Array.prototype.concat()

[`concat()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/concat) 方法用于合并两个或多个数组。此方法不会更改现有数组，而是返回一个新数组。

```js
const arr1 = ['a', 'b', 'c']
const arr2 = ['d', 'e', 'f']

console.log(arr1.concat(arr2)) // ["a", "b", "c", "d", "e", "f"]
```

除了数组作为参数，`concat`也接受其他类型的值作为参数，添加到目标数组尾部。

```js
const arr = []

arr.concat(1, '1', true, {}, null, undefined) // [1, '1', true, {...}, null, undefined]
```

如果数组成员包括对象，`concat` 方法返回当前数组的一个浅拷贝。所谓“浅拷贝”，指的是新数组拷贝的是对象的引用。

## Array.prototype.isArray()

[`isArray()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/isArray) 方法确定传递的值是否为数组。

```js
Array.isArray([1, 2, 3]) // true
Array.isArray({ foo: 123 }) // false
Array.isArray('foobar') // false
Array.isArray(undefined) // false
```

## Array.prototype.reverse()

[`reverse()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reverse) 反转数组的元素顺序。第一个数组元素变为最后一个，最后一个数组元素变为第一个。

```js
const arr = ['one', 'two', 'three']
console.log(arr) // ['one', 'two', 'three']

const reversed = arr.reverse()
console.log(reversed) // ['three', 'two', 'one']
console.log(arr) // ['three', 'two', 'one']
```

> **注意**：该方法会改变原数组。

## Array.prototype.slice()

[`slice()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/slice) 方法选取数组的一部分，并返回一个新数组。原始数组不会被修改。

```js
const animals = ['ant', 'bison', 'camel', 'duck', 'elephant']

console.log(animals.slice(2)) // ["camel", "duck", "elephant"]
console.log(animals.slice(2, 4)) // ["camel", "duck"]
console.log(animals.slice(1, 5)) // ["bison", "camel", "duck", "elephant"]
```

如果 `slice` 方法的参数是负数，则表示倒数计算的位置。

```js
animals.slice(-2) // ['duck', 'elephant']
animals.slice(-2, -1) // ['duck']
```

`slice` 方法的一个重要应用，是将类数组的对象转为真正的数组。

```js
Array.prototype.slice.call({ 0: 'a', 1: 'b', length: 2 }) // ['a', 'b']
```

## Array.prototype.splice()

[`splice()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/splice) 方法通过删除现有元素或添加新元素来更改数组的内容。

```js
const months = ['Jan', 'March', 'April', 'June']
months.splice(1, 0, 'Feb')

// 在第 1 个索引位置插入
console.log(months) // ['Jan', 'Feb', 'March', 'April', 'June']

// 在第 4 个索引处替换 1 个元素
months.splice(4, 1, 'May')
console.log(months) // ['Jan', 'Feb', 'March', 'April', 'May']
```

> **注意**：该方法会改变原数组。

## Array.prototype.indexOf()

[`indexOf()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/indexOf) 方法返回数组中可以找到给定元素的第一个索引，如果该元素不存在，则返回 -1。

```js
const beasts = ['ant', 'bison', 'camel', 'duck', 'bison']

console.log(beasts.indexOf('bison')) // 1

// 从索引 2 开始
console.log(beasts.indexOf('bison', 2)) // 4
console.log(beasts.indexOf('giraffe')) // -1
```

## Array.prototype.lastIndexOf()

[`lastIndexOf()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/lastIndexOf) 方法返回数组中可以找到给定元素的最后一个索引，如果该元素不存在，则返回 -1。从 fromIndex 开始向后搜索数组。

```js
const animals = ['Dodo', 'Tiger', 'Penguin', 'Dodo']

console.log(animals.lastIndexOf('Dodo')) // 3

console.log(animals.lastIndexOf('Tiger')) // 1
```

## Array.prototype.includes()

[`includes()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/includes) 是 ES7 新增的数组方法，用于判断数组是否包含某个元素，并根据条件返回 `true` 或 `false`。

```js
const arr = [10, 11, 3, 20, 5]

const includesTwenty = arr.includes(20)
console.log(includesTwenty) // true
```

`includes` 方法还接受一个可选参数，根据索引来指定要从数组中的哪个位置开始搜索。默认情况下，`index` 参数为零。

```js
const arr = [1, 2, 3, 4, 5]

if (arr.includes(3, 2)) {
  console.log('item found')
} else {
  console.log('item not found')
}
```

> **建议**：使用 `includes` 替代 `indexOf` 检查数组是否包含某个元素。

## Array.prototype.reduce()

[`reduce()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce) 方法根据 reducer 函数和初始值创建任何类型的输出值。

```js
const arr = [1, 2, 3, 4]
const reducer = (acc, cur) => acc + cur

console.log(arr.reduce(reducer)) // 10
console.log(arr.reduce(reducer, 5)) // 15
```

注意，reducer 函数必须显式返回一个值，否则 `reduce()` 将返回 `undefined`。

通过 reducer 函数仅获取 `name` 键：

```js
const input = [
  { name: 'O.O', age: 18 },
  { name: 'D.O', age: 18 },
  { name: 'K.O', age: 18 },
  { name: 'O.K', age: 18 }
]

const reducer = (acc, cur) => {
  acc.push({ name: cur.name })
  return acc
}

let output = input.reduce(reducer, [])
console.log(output)
```

`Reduce()` 支持所以现代浏览器，以及 IE9 及更高版本，如果想支持低版本浏览器，可以添加一个 [polyfill](https://github.com/zloirock/core-js#ecmascript-array) 以支持 IE6。

> 推荐：[Understand JavaScript Reduce With 5 Examples](http://thecodebarbarian.com/javascript-reduce-in-5-examples.html)。

## Array.prototype.reduceRight()

[`reduceRight()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/ReduceRight) 方法对累加器应用一个回调函数，数组的每个值（从右到左）都必须将其减少为单个值。

```js
const arr = [
  [0, 1],
  [2, 3],
  [4, 5]
].reduceRight((acc, cur) => acc.concat(cur))

console.log(arr) // [4, 5, 2, 3, 0, 1]
```

## Array.prototype.filter()

[`filter()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter) 方法使用一个过滤函数创建一个新数组，只保留基于该函数返回 `true` 的元素。结果是一个长度等于或小于原始数组长度的数组，包含与原始数组相同元素的子集。

```js
const array = [10, 11, 3, 20, 5]
const greaterThanTen = array.filter((item) => item > 10)
console.log(greaterThanTen) //[11, 20]
```

**示例**：过滤对象数组

```js
const countries = [
  { name: 'Nigeria', continent: 'Africa' },
  { name: 'Nepal', continent: 'Asia' },
  { name: 'Angola', continent: 'Africa' },
  { name: 'Greece', continent: 'Europe' },
  { name: 'Kenya', continent: 'Africa' },
  { name: 'Greece', continent: 'Europe' }
]

let asianCountries = countries.filter((country) => country.continent === 'Asia')

console.log(asianCountries) // [{name: "Nepal", continent: "Asia"}]
```

**示例**：在数组中搜索特定字母

```js
const names = ['Victoria', 'Pearl', 'Olivia', 'Annabel', 'Sandra', 'Sarah']
const filterItems = (letters) => names.filter((name) => name.includes(letters))

console.log(filterItems('ia')) // ["Victoria", "Olivia"]
```

## Array.prototype.map()

[`map()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map) 方法将提供的函数应用于数组的每个元素并返回一个新数组。

```js
const arr = [1, 2, 3]
const double = (x) => x * 2
arr.map(double) // [2, 4, 6]
```

有一道经典的面试题可以很好的检验您是否理解 `map` 方法：

```js
;['1', '2', '3'].map(parseInt)
```

您可以在 [A JavaScript Optional Argument Hazard](https://wirfs-brock.com/allen/posts/166) 找到问题的解决思路。

## Array.prototype.flat()

[`flat`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/flat) 方法创建一个新数组，其中所有子数组元素递归连接到该数组中，直到指定的深度。

```js
const arr1 = [1, 2, [3, 4]]
arr1.flat() // [1, 2, 3, 4]

const arr2 = [1, 2, [3, 4, [5, 6]]]
arr2.flat() // [1, 2, 3, 4, [5, 6]]

const arr3 = [1, 2, [3, 4, [5, 6]]]
arr3.flat(2) // [1, 2, 3, 4, 5, 6]

const arr4 = [1, 2, [3, 4, [5, 6, [7, 8, [9, 10]]]]]
arr4.flat(Infinity) // [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```

> 更多实现技巧可查阅[数组扁平化](https://github.com/lio-zero/blog/blob/master/JavaScript/%E6%95%B0%E7%BB%84%E6%89%81%E5%B9%B3%E5%8C%96.md)。

## Array.prototype.flatMap()

[flatMap()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/flatMap) 方法首先使用 `map` 方法映射每个元素，然后将结果压缩成一个新数组。它与 `map` 连着深度值为 1 的 `flat` 几乎相同，但 `flatMap` 通常在合并成一种方法的效率稍微高一些。

```js
const arr = [1, 2, 3, 4]

arr.flatMap((x) => [x * 2]) // [2, 4, 6, 8]

// 只有一层是扁平的
arr.flatMap((x) => [[x * 2]]) // [[2], [4], [6], [8]]
```

## Array.prototype.sort()

[`sort()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/sort) 方法对数组中的元素进行适当排序并返回数组。这种情况不一定稳定。默认排序顺序根据字符串 Unicode 代码点。

```js
const months = ['March', 'Jan', 'Feb', 'Dec']
months.sort() // ['Dec', 'Feb', 'Jan', 'March']
console.log(months) // ["Dec", "Feb", "Jan", "March"]

const arr = [1, 30, 4, 21, 100000]
arr.sort()
console.log(arr) // [1, 100000, 21, 30, 4]
```

我们可以向 `sort` 方法传入一个指定按某种顺序进行排列的函数。

```js
arr.sort((a, b) => a - b) // [1, 4, 21, 30, 100000]
arr.sort((a, b) => b - a) // [100000, 30, 21, 4, 1]
```

> 更多内容可查阅[如何在 JavaScript 中对对象数组进行排序？](https://github.com/lio-zero/blog/blob/master/JavaScript/%E5%A6%82%E4%BD%95%E5%9C%A8%20JavaScript%20%E4%B8%AD%E5%AF%B9%E5%AF%B9%E8%B1%A1%E6%95%B0%E7%BB%84%E8%BF%9B%E8%A1%8C%E6%8E%92%E5%BA%8F%EF%BC%9F.md)
>
> **注意**：该方法会改变原数组。

## Array.prototype.every()

[`every()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/every) 方法对数组中每一项运行给定函数，如果函数对每一项都返回 `true`，则返回 `true`。

```js
const arr = [1, 2, 3, 4, 5]

const isBelowThreshold = (currentValue) => currentValue < 10
console.log(arr.every(isBelowThreshold)) // true
```

## Array.prototype.some()

[`some()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/some) 方法对数组中每一项运行给定函数，如果函数对任一项返回 `true`，则返回 `true`。

```js
const arr = [1, 2, 3, 4, 5]

// 检查元素是否为偶数
const even = (item) => item % 2 === 0
console.log(arr.some(even)) // true
```

**示例**：数组扁平化

```js
let arr = [1, 2, [3, 4, 5, [6, 7], 8], 9, 10, [11, [12, 13]]]

const flatten = function (arr) {
  // 只要有一个元素为数组，那么循环继续
  while (arr.some(Array.isArray)) {
    arr = [].concat(...arr)
  }
  return arr
}

console.log(flatten(arr))
```

## Array.prototype.forEach()

[`forEach()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach) 方法对每个数组元素执行一次提供的回调函数。

```js
const arr = ['a', 'b', 'c']

arr.forEach((item) => console.log(item))

// "a"
// "b"
// "c"
```

## Array.prototype.find()

[`find()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/find) 方法返回匹配器函数为其返回 `true` 的第一个元素。否则返回 -1。其结果是原始数组中的一个元素。

```js
const arr = [10, 11, 3, 20, 5]

const greaterThanTen = arr.find((item) => item > 10)
console.log(greaterThanTen) // 11
```

## Array.prototype.findIndex()

[`findIndex()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/findIndex) 方法返回数组中满足提供的测试函数的第一个元素的索引。否则返回 -1。

```js
const arr = [5, 12, 8, 130, 44]

const isLargeNumber = (item) => item > 13
console.log(arr.findIndex(isLargeNumber)) // 3
```

## Array.prototype.findLast/findLastIndex

通过 `find` 和 `findIndex` 在数组中查找值是一种常见的做法。不过，这些方法从数组开头开始迭代。

```js
const things = [{ v: 1 }, { v: 2 }, { v: 3 }, { v: 4 }, { v: 5 }]

things.find((item) => item.v > 3) // {v: 4}
things.findIndex((item) => item.v > 3) // 3
```

如果您想从数组的末尾开始搜索数组，则必须反转数组并使用提供的方法。这并不好，因为它需要不必要的数组突变。

幸运的是，有一个针对 `findLast` 和 `findLastIndex` 的 ECMAScript 提案。

```js
const things = [{ v: 1 }, { v: 2 }, { v: 3 }, { v: 4 }, { v: 5 }]

things.findLast((item) => item.v > 3) // {v: 5}
things.findLastIndex((item) => item.v > 3) // 4
```

该提案目前处于第三阶段，目前已经 Edge、chromium 和 Safari 的最新版本得到支持。至于其余的，[core-js 和 Babel 已经提供了一个 polyfill](https://github.com/zloirock/core-js/blob/master/packages/core-js/internals/array-iteration-from-last.js)。

以下是 `findLast` 的支持情况，`findLastIndex` 与其相同：

![findLast 的支持情况](https://upload-images.jianshu.io/upload_images/18281896-f397049f25922096.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## Array.prototype.toLocaleString()

[`toLocaleString()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/toLocaleString) 返回一个字符串表示数组中的元素。数组中的元素将使用各自的 toLocaleString 方法转成字符串，这些字符串将使用一个特定语言环境的字符串（例如一个逗号 ","）隔开。

```js
const arr = [1, 'a', new Date('21 Dec 1997 14:12:00 UTC')]
const localeString = arr.toLocaleString('en', { timeZone: 'UTC' })

console.log(localeString) // "1,a,12/21/1997, 2:12:00 PM"
// 这假设 'en' 语言环境和 UTC 时区 - 您的结果可能会有所不同
```

## Array.prototype.keys()

[`keys()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/keys) 方法返回一个新的数组可迭代对象，包含原始数组中每个索引的键。

```js
const arr = ['a', 'b', 'c']

for (let key of arr.keys()) {
  console.log(key) // 0 1 2
}
```

## Array.prototype.values()

[`values()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/values) 方法返回一个新的数组迭代器对象，该对象包含数组中每个索引的值。

```js
const arr = ['a', 'b', 'c']

for (const value of arr.values()) {
  console.log(value) // "a" "b" "c"
}
```

## Array.prototype.entries()

[`entries()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/entries) 方法返回一个新的数组可迭代对象，该对象包含数组中每个索引的键/值对。

```js
const arr = ['a', 'b', 'c']
const iterator = arr.entries()

console.log(iterator.next().value) // [0, "a"]

console.log(iterator.next().value) // [1, "b"]
```

## Array.prototype.fill()

[`fill()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/fill) 方法使用一个固定值来填充从起始索引（默认 `0`）到结束索引（默认 `array.length`）的数组的所有元素。它返回修改后的数组。

```js
const arr = [1, 2, 3, 4]

// 从位置 2 到位置 4 填充 0
console.log(arr.fill(0, 2, 4)) // [1, 2, 0, 0]

// 从位置 1 填充 5
console.log(arr.fill(5, 1)) // [1, 5, 5, 5]

console.log(arr.fill(6)) // [6, 6, 6, 6]

Array(3) // [empty × 3]
const money = () => '🤑'
[...Array(3)].map(money) // ['🤑', '🤑', '🤑']
Array(3).fill('🤑') // ['🤑', '🤑', '🤑']
```

**示例**：重复字符串

```js
Array(3).fill('LOVE').join('')

// "LOVELOVELOVE"
```

该片段的运行很简单：`Array(3)` 创建一个包含 3 个空项的新数组，`fill('LOVE')` 在每个空项中填充 LOVE，`join()` 将所有元素连接成一个字符串。

> **注意**：该方法会改变原数组。

## Array.prototype.copyWithin()

[`copyWithin()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/copyWithin) 方法将数组的一部分复制到同一数组中的另一个位置并返回它，而不修改其大小。

```js
const arr = ['a', 'b', 'c', 'd', 'e']

// 将索引 3 处的元素复制到索引 0
console.log(arr.copyWithin(0, 3, 4)) // ["d", "b", "c", "d", "e"]

// 将从索引 3 到末尾的所有元素复制到索引 1
console.log(arr.copyWithin(1, 3)) // ["d", "d", "e", "d", "e"]
```

> **注意**：该方法会改变原数组。

## Array.prototype.of()

[`Array.of()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/of) 方法从数量可变的参数创建一个新的数组实例，而不管参数的数量或类型如何。

```js
Array.of(7) // [7]
Array(7) // [empty × 7]，7个空插槽的数组

Array.of(1, 2, 3) // [1, 2, 3]
Array(1, 2, 3) // [1, 2, 3]
```

`Array.of()` 和数组构造函数之间的区别在于处理整数参数：`Array.of(7)` 创建一个包含单个元素 7 的数组，而 `Array(7)` 创建一个长度属性为 7 的空数组（**注意**：这意味着数组包含 7 个空插槽，而不是包含实际未定义值的插槽）。

## Array.prototype.from()

[`Array.from()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/from) 用于将类数组对象或可迭代对象转化为一个新的浅拷贝数组实例。

```js
console.log(Array.from('foo')) // ["f", "o", "o"]
console.log(Array.from([1, 2, 3], (x) => x + x)) // [2, 4, 6]
```

`Array.from()` 接受第二个 `map` 参数。方便创建和填充特定长度的数组。

![使用 Array.from 填充特定长度](https://upload-images.jianshu.io/upload_images/18281896-1498e669acad1b28.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

ES6 `Set`是可以相互转换的，可以使用 `Array.from` 转换成数组

```js
const article = new Set(['JavaScript', 'TypeScript', 'Node.js'])

article // Set(3) {'JavaScript', 'TypeScript', 'Node.js'}
Array.from(article) // ['JavaScript', 'TypeScript', 'Node.js']
```

**示例**：重复字符串

一个类数组对象必须含有 `length` 属性，且元素属性名必须是数值或者可转换为数值的字符。我们正好可以利用这一点。

我们将 `length` 属性设置为 3，使用 `from()` 方法初始化数组，每个位置对应 1 个 `undefined`。 如下：

```js
Array.from({ length: 3 }) // [undefined, undefined, undefined]
```

现在我们使用第二个参数，这是一个 `map` 函数，它将调用数组的每个元素。在这里，我们用 `'LOVE'` 替换所有的 `undefined`。

```js
Array.from({ length: 3 }, () => 'LOVE') // ['LOVE', 'LOVE', 'LOVE']
```

最后我们调用 `join()` 方法将数组中的所有元素组合成一个字符串。

```js
Array.from({ length: 3 }, () => 'LOVE').join('') //'LOVELOVELOVE'
```

**示例**：分块

在使用 API 时，我经常需要将用户列表分块并批量发送。`Array.from()` 是一种将数组分块的好方法，因为第二个参数是一个 `map` 函数。

```js
function chunkify(array, chunkSize = 10) {
  const chunks = Array.from(
    { length: Math.ceil(array.length / 10) },
    (_, i) => {
      const start = chunkSize * i
      return array.slice(start, start + chunkSize)
    }
  )
  return chunks
}

chunkify(hugeArray) // [[...], [...], ....]
```

## Array.prototype.toString()

[`toString()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/toString) 方法返回一个字符串，表示指定的数组及其元素。

```js
const arr = [1, 2, 'a', 'b']
console.log(arr.toString()) // "1,2,a,b"

const arr = [1, 2, 3, [4, 5, 6]]
console.log(arr.toString()) // "1,2,3,4,5,6"
```

## Array.prototype.valueOf()

[`valueOf()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/valueOf) 方法返回指定对象的原始值。

不同对象的 `valueOf` 方法不尽一致，数组的 `valueOf` 方法返回数组本身。

```js
const arr = [1, 2, 3]
arr.valueOf() // [1, 2, 3]
```

> 详细查看 MDN 上 [`valueOf` 各个类型的示例](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/valueOf#%E4%BD%BF%E7%94%A8_valueof)。

## Array.prototype.at()

[`at()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/at) 方法接收一个整数值并返回该索引的项目，允许正数和负数。负整数从数组中的最后一个项目开始倒数。

语法：

```js
at(index)
```

### 通过索引获取数组项

以下是一个示例，假设我们有以下数组：

```js
let arr = ['O.O', 'D.O', 'K.O', 'O.K', 'KK']
```

使用括号表示法，可以通过其索引获取项：

```js
arr[2] // 'K.O'

arr[4] // 'KK'
```

使用 `at()` 方法可以执行相同的操作。在数组中调用它，并传入所需的索引，它返回匹配项。

```js
arr.at(2) // 'K.O'

arr.at(4) // 'KK'
```

如果要从数组末尾获取项，通过括号表示法，需要获取数组的长度，从想要的末尾减去数字，然后将其传入。

```js
arr[arr.length - 2] // 'O.K'
```

而使用 `at()` 方法，只需将索引作为负整数传递。

```js
arr.at(-2) // 'O.K'
```

## 有无 `mutation`

**`mutation` 译为突变，可以简单理解为是否改变原数组，有无副作用。**

**mutation**:

- `splice()`、`copyWithin()`、`fill()`、`pop()`、`push()`
- `reverse()`、`shift()`、`sort()`、`unshift()`

**no mutation**:

- `some()`、`map()`、`forEach()`、`every()`、`filter()`、`reduce()`、`reduceRight()`
- `find()`、`findIndex()`、`lastIndexOf()`、`includes()`、`indexOf()`、`findLast()`、`findLastIndex()`
- `join()`、`slice()`、`concat()`、`flat()`、`flatMap()`、`of()`、`from()`
- `toLocaleString()`、`toSource()`、`toString()`、`valueOf()`、`keys()`、`values()`、`entries()`

## 根据特定条件搜索

上面已经介绍了数组的所有方法，我们可以使用一些数组方法来代替 `for` 循环来搜索数组。根据需要，您可以决定哪种方法最适合您的用例。

- 如果要查找数组中满足特定条件的所有项，请使用 `filter`。
- 如果要检查至少一项是否满足特定条件，请使用 `find`。
- 如果要检查数组是否包含特定值，请使用 `includes`。
- 如果要查找数组中特定项的索引，请使用 `indexOf`。
- 如果要检查数组中所有元素是否满足给定条件，并且返回一个布尔值，请使用 `every`。
- 如果要检查数组中任一元素是否满足给定条件，并且返回一个布尔值，请使用 `some`。

## concat 和 push 的区别

`concat` 和 `push` 是将一个或多个项附加到给定数组的常用方法。

### 区别

`concat` 方法不会更改现有数组：

```js
const shapes = ['Triangle', 'Square']

shapes.concat(['Circle', 'Rectangle']) // ['Triangle', 'Square', 'Circle', 'Rectangle']
shapes // ['Triangle', 'Square']
```

另一方面，`push` 方法会修改原始数组。

```js
const animals = ['cat', 'dog', 'mouse']
animals.push('bird', 'cow') // 5
animals // ['cat', 'dog', 'mouse', 'bird', 'cow']
```

在上面的示例代码中，两种方法产生不同的结果。`concat` 返回一个新数组，`push` 返回更新数组的长度。

因为 `concat` 创建了一个新数组来保存合并数组的结果，所以它比 `push` 慢。

对于小型数组，这两种方法在性能方面不会产生显著差异。但是如果你必须使用大数组，性能是应用程序的关键，那么考虑使用 `push`。

### 提示

您可以使用 ES6 扩展运算符将不同的数组合并为一个：

```js
const a = [1, 2, 3]
const b = [4, 5, 6]
const c = [7, 8, 9]

const final = [...a, ...b, ...c] // [1, 2, 3, 4, 5, 6, 7, 8, 9]
```

如果我们也使用 `push` 方法，则扩展运算符很方便：

```js
const a = [1, 2, 3]
const b = [4, 5, 6]

a.push(...b) // 6
a // [1, 2, 3, 4, 5, 6]
```

`Array.prototype.push.apply(firstArray, secondArray)` 是另一个同样适用于 ES5 的选项。但是，如果第二个数组非常大，则不建议使用这种方法，因为一个函数中的最大参数数是有限的。

```js
Array.prototype.push.apply(a, b) // 6
a // [1, 2, 3, 4, 5, 6]
```

如果你好奇的话，它的硬编码是 [65536](https://bugs.webkit.org/show_bug.cgi?id=80797)。

[JavaScript Array.push is 945x faster than Array.concat 🤯🤔](https://dev.to/uilicious/javascript-array-push-is-945x-faster-than-array-concat-1oki) 提供了许多基准来证明为什么 `push` 比 `concat` 更快。

## `slice` 和 `splice` 的区别

`slice` 和 `splice` 是获取给定数组子数组的常用方法。

### 区别

语法：

```js
array.slice(startingIndex, endingIndex)
array.splice(startingIndex, length, ...items)
```

虽然第一个参数彼此相同，表示删除元素的起始索引，但第二个参数不同。

`slice` 和 `splice` 的第二个参数分别是结束索引和子项数。

`splice` 更改原始数组，而 `slice` 不更改。

给定从 1 到 5 的数字数组：

```js
const arr = [1, 2, 3, 4, 5]
const sub = arr.splice(2, 3)

// 原始数组已修改
arr // [1, 2]
sub // [3, 4, 5]
```

现在，让我们将相同的参数传递给 `slice`：

```js
const arr = [1, 2, 3, 4, 5]
const sub = arr.slice(2, 3)

// 原始数组未被修改
arr // [1, 2, 3, 4, 5]
sub // [3]
```

使用 `splice` 方法，可以通过将项目传递到最后一个参数来保持项目不从原始数组中移除。

```js
const arr = [1, 2, 3, 4, 5]
const sub = arr.splice(2, 3, 3, 4, 5)

arr // [1, 2, 3, 4, 5]
```

### 提示

我们可以通过忽略结束索引来克隆数组（浅拷贝）：

```js
const clone = (arr) => arr.slice(0)
```

> 更多详细内容可查阅[浅拷贝和深拷贝](https://github.com/lio-zero/blog/blob/master/JavaScript/%E6%B5%85%E6%8B%B7%E8%B4%9D%E5%92%8C%E6%B7%B1%E6%8B%B7%E8%B4%9D.md)

## `some` 和 `every`

这两个方法类似**断言**（assert），返回一个布尔值，表示判断数组成员是否符合某种条件。

在前面方法的介绍中，我们已经大致了解了 `every` 和 `some` 方法的工作原理，下面是您可以从中获得的一些附加功能：

- `Array.every` 和 `Array.some` 可以接受第二个参数，该参数将在回调函数的执行中用作 `this` 参数。
- 回调函数可以接受两个额外的参数：当前项的索引和对调用该方法的数组的引用。

来看看一个示例：

```js
const obj = { name: 'Daffodil' }
const arr = [1, 2, 3, 4, 5, 6]

arr.every(function (el, i, arr) {
  return el > i && arr[i] === el && this === obj
}, obj) // true
```

- 每个元素都大于其索引
- `arr[i]`（其中 `i` 当前索引与当前元素相同）
- 我们提供了 `obj` 作为 `this` 参数的引用，因此 `this` 等于 `obj`

> **注意**：对于空数组，`some` 方法返回 `false`，`every` 方法返回 `true`，回调函数都不会执行。

```js
const isEven = x => x % 2 === 0

[].some(isEven) // false
[].every(isEven) // true
```

`some` 和 `every` 方法还可以接受第二个参数，用来绑定参数函数内部的 `this` 变量。

## 通过索引获取元素

```js
const list = [1, 2, 3, 4, 5]
list[1] // 2
list.indexOf(2) // 1
```

## 通过索引获取数组的子集

**不改变原数组**：

```js
list.slice(0, 1) // [1]
list.slice(1) // [2, 3, 4, 5]
list.slice(1, 2) // [2]
```

**改变原数组**：

```js
re = list.splice(1) // re = [2, 3, 4, 5]  list == [1]
re = list.splice(1, 2) // re = [2, 3]      list == [1, 4, 5]
```

## 删除元素

```js
const list = [1, 2, 3, 4, 5]
list.pop() // 5    list == [1, 2, 3, 4]
list.shift() // 1    list == [2, 3, 4, 5]
list.splice(2, 1) // [3]  list == [1, 2, 3, 4]
```

## 替换元素

```js
const list = [1, 2, 3, 4, 5]

list.splice(2, 1, 6) // list == [1, 2, 6, 4, 5]
```

## 插入

```js
const list = [1, 2, 3, 4, 5]

// 指定元素之后插入
list.splice(list.indexOf(2) + 1, 0, 1) // [1, 2, 1, 3, 4, 5]

// 指定元素之前插入
list.splice(list.indexOf(2), 0, 6) // [1, 6, 2, 1, 3, 4, 5]
```

## 添加元素

不改变原数组：

```js
list.concat([6]) // [1, 2, 3, 4, 5, 6]
```

改变原数组：

```js
list.push(6) // list == [1, 2, 3, 4, 5, 6]
list.unshift(6) // list == [6, 5, 4, 3, 2, 1]
list.splice(2, 0, 6) // list == [1, 2, 6, 3, 4, 5]
```

## 更多资料

- [Array](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array)
- [数组方法](https://zh.javascript.info/array-methods)
- [30 Seconds Of Code 有大量关于数组的数组片段和文章](https://www.30secondsofcode.org/js/t/array/p/1)

以下整理了过往写过关于数组的文章：

- [如何在 JavaScript 中对对象数组进行排序？](https://github.com/lio-zero/blog/blob/master/JavaScript/%E5%A6%82%E4%BD%95%E5%9C%A8%20JavaScript%20%E4%B8%AD%E5%AF%B9%E5%AF%B9%E8%B1%A1%E6%95%B0%E7%BB%84%E8%BF%9B%E8%A1%8C%E6%8E%92%E5%BA%8F%EF%BC%9F.md)
- [数组扁平化](https://github.com/lio-zero/blog/blob/master/JavaScript/%E6%95%B0%E7%BB%84%E6%89%81%E5%B9%B3%E5%8C%96.md)
- [JavaScript 数组去重](https://github.com/lio-zero/blog/blob/master/JavaScript/JavaScript%20%E6%95%B0%E7%BB%84%E5%8E%BB%E9%87%8D.md)
- [如何在 JavaScript 中合并两个数组？](https://github.com/lio-zero/blog/blob/master/JavaScript/%E5%A6%82%E4%BD%95%E5%9C%A8%20JavaScript%20%E4%B8%AD%E5%90%88%E5%B9%B6%E4%B8%A4%E4%B8%AA%E6%95%B0%E7%BB%84%EF%BC%9F.md)
- [如何在 JavaScript 中判断一个值是否为数组？](https://github.com/lio-zero/blog/blob/master/JavaScript/%E5%A6%82%E4%BD%95%E5%9C%A8%20JavaScript%20%E4%B8%AD%E5%88%A4%E6%96%AD%E4%B8%80%E4%B8%AA%E5%80%BC%E6%98%AF%E5%90%A6%E4%B8%BA%E6%95%B0%E7%BB%84%EF%BC%9F.md)
- [如何在 JavaScript 中将数组转为对象](https://github.com/lio-zero/blog/blob/master/JavaScript/%E5%A6%82%E4%BD%95%E5%9C%A8%20JavaScript%20%E4%B8%AD%E5%B0%86%E6%95%B0%E7%BB%84%E8%BD%AC%E4%B8%BA%E5%AF%B9%E8%B1%A1.md)
- [检查数组是否已排序](https://github.com/lio-zero/blog/blob/master/JavaScript/%E6%A3%80%E6%9F%A5%E6%95%B0%E7%BB%84%E6%98%AF%E5%90%A6%E5%B7%B2%E6%8E%92%E5%BA%8F.md)
- [过滤并排序字符串列表](https://github.com/lio-zero/blog/blob/master/JavaScript/%E8%BF%87%E6%BB%A4%E5%B9%B6%E6%8E%92%E5%BA%8F%E5%AD%97%E7%AC%A6%E4%B8%B2%E5%88%97%E8%A1%A8.md)
- [数组平均值与中位数](https://github.com/lio-zero/blog/blob/master/JavaScript/%E6%95%B0%E7%BB%84%E5%B9%B3%E5%9D%87%E5%80%BC%E4%B8%8E%E4%B8%AD%E4%BD%8D%E6%95%B0.md)
- [从数组中删除重复的对象](https://github.com/lio-zero/blog/blob/master/JavaScript/%E4%BB%8E%E6%95%B0%E7%BB%84%E4%B8%AD%E5%88%A0%E9%99%A4%E9%87%8D%E5%A4%8D%E7%9A%84%E5%AF%B9%E8%B1%A1.md)
- [数组中的最大值/最小值](https://github.com/lio-zero/blog/blob/master/JavaScript/%E6%95%B0%E7%BB%84%E4%B8%AD%E7%9A%84%E6%9C%80%E5%A4%A7%E5%80%BC-%E6%9C%80%E5%B0%8F%E5%80%BC.md)
- [JavaScript 中清空数组](https://github.com/lio-zero/blog/blob/master/JavaScript/JavaScript%20%E4%B8%AD%E6%B8%85%E7%A9%BA%E6%95%B0%E7%BB%84.md)
- [如何在 JavaScript 中将数组拆分为一组数组](https://github.com/lio-zero/blog/blob/master/JavaScript/%E5%A6%82%E4%BD%95%E5%9C%A8%20JavaScript%20%E4%B8%AD%E5%B0%86%E6%95%B0%E7%BB%84%E6%8B%86%E5%88%86%E4%B8%BA%E4%B8%80%E7%BB%84%E6%95%B0%E7%BB%84.md)
- [如何在 JavaScript 中判断数组是否包含某个值？](https://github.com/lio-zero/blog/blob/master/JavaScript/%E5%A6%82%E4%BD%95%E5%9C%A8%20JavaScript%20%E4%B8%AD%E5%88%A4%E6%96%AD%E6%95%B0%E7%BB%84%E6%98%AF%E5%90%A6%E5%8C%85%E5%90%AB%E6%9F%90%E4%B8%AA%E5%80%BC%EF%BC%9F.md)
