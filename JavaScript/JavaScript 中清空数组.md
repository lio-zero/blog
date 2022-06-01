# JavaScript 中清空数组

## Array.prototype.splice()

使用 `Array.prototype.splice()` 方法通过删除或替换现有元素来修改原数组的内容。

```js
let course = ['HTML', 'CSS', 'JavaScript']

course.splice(0)
console.log(course.length) // 0
```

> **注意**：此方法会影响其他引用。

```js
let course = ['HTML', 'CSS', 'JavaScript']
let other = course

course.splice(0)
course // []
other // []
```

由于 `splice()` 返回一个已删除项目的数组，您可以通过将结果分配给一个新变量来获取原始数组的副本：

```js
let foo = ['hello', 'world']

// 清空并创建一个 foo 的副本
let bar = foo.splice(0, foo.length)

console.log(foo) // []
console.log(bar) // ['hello', 'world']
```

## 将 length 设置为 0

将数组的长度设置为零。

```js
let course = ['HTML', 'CSS', 'JavaScript']
course.length = 0
console.log(course) // []
```

## 分配新的空数组

将新的空数组分配给现有数组也会使数组为空。

```js
let course = ['HTML', 'CSS', 'JavaScript']
course = []
console.log(course.length) // 0
```

## pop/shift

循环判断数组的 `length` 属性，在每次循环都移除一个元素。

```js
let course = ['HTML', 'CSS', 'JavaScript']

while (course.length > 0) {
  course.pop()
  // course.shift()
}

console.log(course) // 0
```

## length = 0 和 Array = [] 之间的差异？

在大多数情况下，将变量重新分配给空数组是更好的选择。它比调整 `length` 属性更短、更明确。

但有时，你有一个数组是通过引用分配的，你想让它们保持引用。

假如我们有一个 `foo` 数组，我们还有一个 `bar` 变量，我将它的值设置为 `foo` 数组。

```js
let foo = ['hello', 'world']

// 添加引用
let bar = foo
```

如果我重新分配 `foo` 的值为 `[]` 空数组，`bar` 变量仍指向分配给它的原始数组。

```js
foo = []

// bar 不受影响
console.log(bar) // ['hello', 'world']
```

`foo = []` 将一个新的数组的引用赋值给变量，其他引用并不受影响。这意味着以前数组的内容被引用的话将依旧存在于内存中，这将导致内存泄漏。

如果我改为 `foo.length = 0`，删除数组里的所有内容，也将影响到其他引用。

```js
let foo = ['hello', 'world']
let bar = foo

foo.length = 0

// `bar` 受影响
console.log(bar) // []
```

如果数组被声明为常量，则不能将其重新分配给 `[]`。这时我们应该使用 `lenght = 0`。

```js
const course = ['HTML', 'CSS', 'JavaScript']
course = [] // 会抛出异常："Assignment to constant variable"

// 使用 length = 0
course.length = 0
```

## 更多资料

你可以在 [How do I empty an array in JavaScript?](https://stackoverflow.com/questions/1232040/how-do-i-empty-an-array-in-javascript) 和 [Difference between Array.length = 0 and Array =[]?](https://stackoverflow.com/questions/4804235/difference-between-array-length-0-and-array) 查看更多详细的内容，比如这些方法的性能测试，进行讨论。
