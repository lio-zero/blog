# HTMLCollection 和 NodeList 的区别

## HTMLCollection

> [`HTMLCollection`](https://developer.mozilla.org/en-US/docs/Web/API/HTMLCollection) 表示一个包含了元素（元素顺序为文档流中的接口）的集合（通用集合），还提供了从该集合中选择元素的属性和方法。

例如，使用 `getElementsByTagName()` 方法返回的就是一个 HTMLCollection 对象。

HTMLCollection 对象中的属性和方法：

- `item(index)` — 返回 HTMLCollection 中指定索引的元素，不存在返回 `null`。
- `length`（只读）— 返回 HTMLCollection 中元素的数量。

```js
document.getElementsByTagName('body') instanceof HTMLCollection // true

const htmlCollection = document.getElementsByTagName('body')
console.log(htmlCollection.item(0)) // <body>...</body>
console.log(htmlCollection.length()) // 1
```

## NodeList

[`NodeList`](https://developer.mozilla.org/en-US/docs/Web/API/NodeList) 对象是节点的集合。它可以通过以下方法得到：

- 一些**旧版本浏览器**中的方法（如 `getElementsByClassName()`），返回的是 NodeList 对象，而不是 HTMLCollection 对象。
- 所有浏览器的 `Node.childNodes` 属性返回的是 NodeList 对象。
- 大部分浏览器的 `document.querySelectorAll()` 返回 NodeList 对象。

```js
document.body.childNodes instanceof NodeList // true
document.querySelectorAll('body') instanceof NodeList // true or false
document.getElementsByClassName('body') instanceof NodeList // false or true
```

NodeList 对象中的属性和方法：

- `item()` — 返回某个元素基于文档树的索引
- `length` — 返回 NodeList 的节点数量。
- `NodeList.forEach()` 方法用于遍历 NodeList 的所有成员。它接受一个回调函数作为参数，每一轮遍历就执行一次这个回调函数，用法与数组实例的 `forEach` 方法完全一致。
- `NodeList.keys()/values()/entries()` — 这三个方法都返回一个 ES6 的遍历器对象，可以通过 `for...of` 循环遍历获取每一个成员的信息。

**区别**：`keys()` 返回键名的遍历器，`values()` 返回键值的遍历器，`entries()` 返回的遍历器同时包含键名和键值的信息。如果你还不熟悉，可以在我之前写过的一篇 [Object.keys/values/entries](https://github.com/lio-zero/blog/blob/master/JavaScript/Object.keys%E3%80%81values%E3%80%81entries.md) 了解它基本用法。

```js
const nodeList = document.querySelectorAll('body')

console.log(nodeList.item(0)) // <body>...</body>
console.log(nodeList.length) // 1
console.log(nodeList.forEach((item) => console.log(item))) // <body>...</body>

for (var key of nodeList.keys()) {
  console.log(nodeList[key]) // <body>...</body>
}

for (var value of nodeList.values()) {
  console.log(value) // <body>...</body>
}

for (var entry of nodeList.entries()) {
  console.log(entry) // [0, body]
}
```

## 将 NodeList 转换为数组

`document.querySelectorAll` 方法返回一个类数组对象称为 NodeList。这些数据结构被称为 “类数组”，因为他们看似数组却没有类似 `map`、`forEach` 这样的数组方法。

你可以通过下面几种方法将 NodeList 转换为 DOM 元素的数组：

- `Array.from`

```js
const nodeList = document.querySelectorAll('div')
Array.from(nodeList)
```

- `apply`、`call` 和 `bind`

```js
const nodeList = document.querySelectorAll('div')
Array.apply(null, nodeList)
```

另外，你可以使用 `Array.prototype.slice` 结合 `Function.prototype.call`、`Function.prototype.apply` 或 `Function.prototype.bind`， 将类数组对象当做 `this` 传入：

```js
Array.prototype.slice.call(nodeList)
Array.prototype.slice.apply(nodeList)
Array.prototype.slice.bind(nodeList)()
```

更简便的方法是使用 ES6 [扩展运算符（`...`）](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Spread_operator)

```js
const nodeList = [...document.querySelectorAll('div')]
nodeList
```

## HTMLCollection 与 NodeList 的区别

- NodeList 是一个静态集合，其不受 DOM 树元素变化的影响；相当于是 DOM 树快照，节点数量和类型的快照，就是对节点增删，NodeList 感觉不到。但是对节点内部内容修改，是可以感觉到的，比如修改 `innerHTML`。
- HTMLCollection 是动态绑定的，是一个的动态集合，DOM 树发生变化，HTMLCollection 也会随之变化，节点的增删是敏感的。
- 只有 NodeList 对象有包含属性节点和文本节点。
- HTMLCollection 元素可以通过 `name`，`id` 或 `index` 索引来获取。NodeList 只能通过 `index` 索引来获取。
- HTMLCollection 和 NodeList 本身无法使用数组的方法：`pop()`、`push()` 或 `join()` 等。除非你把他转为一个数组，上文有介绍到。

## querySelectorAll 和 getElementsByTagName 的区别

`querySelectorAll` 返回 `NodeList` 集合，而 `getElementsByTagName` 返回 `HTMLCollection` 集合。

从上面的 HTMLCollection 与 NodeList 的区别可以知道，HTMLCollection 是一个动态集合，DOM 树变化，HTMLCollection 也会随着变化。而 NodeList 是一个静态集合，其不受 DOM 树元素变化的影响。

下面是两个例子：

```html
<ul>
  <li></li>
  <li></li>
  <li></li>
  <li>🤣</li>
  <li></li>
</ul>
```

NodeList 示例：

```js
const oldUl = document.querySelectorAll('ul')[0]
const oldLi = document.querySelectorAll('li')

console.log(oldLi) // NodeList(5) [li, li, li, li, li]
console.log(oldLi.length) // 5

const newLi = document.createElement('li')
oldUl.appendChild(newLi)

console.log(oldLi.length) // 5
oldLi[oldLi.length - 1].innerHTML = '🤐'
console.log(oldLi[oldLi.length - 1].innerHTML) //  🤐
```

可以看到，使用 `querySelectorAll` 获取 `<li>` 返回的是一个 NodeList 集合。页面 DOM 结构发生改变，其长度不会发生任何变化，而改变 `innerHTML`，则相应发生改变。

HTMLCollection 示例：

```js
const oldUl = document.querySelectorAll('ul')[0]
const oldLi = document.getElementsByTagName('li')

console.log(oldLi) // HTMLCollection(5) [li, li, li, li, li]
console.log(oldLi.length) // 5

const newLi = document.createElement('li')
oldUl.appendChild(newLi)

console.log(oldLi.length) // 6
```

可以看到，使用 `getElementsByTagName` 获取 `<li>` 返回的是一个 HTMLCollection 集合。页面 DOM 结构发生改变，将动态获取其长度。

## 哪种类型更好

通常，拥有一个保持不变的列表是一件好事。但有时您可能需要一个自动更新以反映当前 UI 的列表。

了解选择器方法是返回静态集合还是动态集合也有助于防止意外的副作用和错误。
