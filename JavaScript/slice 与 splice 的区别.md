# slice 与 splice 的区别

`slice` 和 `splice` 是获取给定数组子数组的常用方法。

## 区别

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

## 提示

我们可以通过忽略结束索引来克隆数组（浅拷贝）：

```js
const clone = (arr) => arr.slice(0)
```
