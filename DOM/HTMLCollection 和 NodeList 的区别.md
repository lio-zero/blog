# HTMLCollection 和 NodeList 的区别

HTMLCollection 和 NodeList 是 JavaScript 中两种不同的对象类型，它们都用于表示多个 DOM 元素。

## HTMLCollection

[`HTMLCollection`](https://developer.mozilla.org/en-US/docs/Web/API/HTMLCollection) 表示一个包含了元素的集合，还提供了从该集合中选择元素的属性和方法。

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

[`NodeList`](https://developer.mozilla.org/en-US/docs/Web/API/NodeList) 对象是节点的集合。

NodeList 对象可以通过 `Node.childNodes` 属性和 `document.querySelectorAll()` 方法获取。

```js
document.body.childNodes instanceof NodeList // true
document.querySelectorAll('body') instanceof NodeList // true
```

NodeList 对象中的属性和方法：

- `item()` — 返回某个元素基于文档树的索引。
- `length` — 返回 NodeList 的节点数量。
- `NodeList.forEach()` 方法用于遍历 NodeList 的所有成员。它接受一个回调函数作为参数，每一轮遍历就执行一次这个回调函数，用法与数组实例的 `forEach` 方法完全一致。
- `NodeList.keys()/values()/entries()` — 这三个方法都返回一个 ES6 的遍历器对象，可以通过 `for...of` 循环遍历获取每一个成员的信息。

**区别**：`keys()` 返回键名的遍历器，`values()` 返回键值的遍历器，`entries()` 返回的遍历器同时包含键名和键值的信息。如果你还不熟悉，可以在我之前写过的一篇 [Object.keys/values/entries](https://github.com/lio-zero/blog/blob/master/JavaScript/Object.keys%E3%80%81values%E3%80%81entries.md) 了解它基本用法。

```js
const nodeList = document.querySelectorAll('body')

console.log(nodeList.item(0)) // <body>...</body>
console.log(nodeList.length) // 1
console.log(nodeList.forEach(console.log)) // <body>...</body>

for (let key of nodeList.keys()) {
  console.log(nodeList[key]) // <body>...</body>
}

for (let value of nodeList.values()) {
  console.log(value) // <body>...</body>
}

for (let entry of nodeList.entries()) {
  console.log(entry) // [0, body]
}
```

## 将 NodeList 或 HTMLCollection 转换为数组

HTMLCollection 和 NodeList 是类数组对象，本身无法使用数组的方法。除非你把它转为一个数组。

详细的类数组转数组可以查看：[类数组对象转数组](https://github.com/lio-zero/blog/blob/main/JavaScript/%E7%B1%BB%E6%95%B0%E7%BB%84%E5%AF%B9%E8%B1%A1%E8%BD%AC%E6%95%B0%E7%BB%84.md)。

## HTMLCollection 与 NodeList 的区别

- HTMLCollection 是一个的动态集合，如果 DOM 树发生变化，HTMLCollection 也会随之变化，节点的增删是敏感的。
- 在大部分情况下，NodeList 是一个静态集合，也就意味着随后对 DOM 的任何改动都不会影响集合的内容，比如 `document.querySelectorAll` 就会返回一个静态 NodeList（注意内容的修改会是实时）。但存在一个例外，DOM 树的变化会反映在 `Node.childNodes` 属性中，这时 NodeList 可以是一个动态集合。
- HTMLCollection 元素可以通过 `name`、`id` 或 `index` 索引来获取。NodeList 只能通过 `index` 索引来获取。
- 只有 NodeList 对象有包含属性节点和文本节点。

注意

## querySelectorAll 和 getElementsByTagName 的区别

`querySelectorAll` 返回 `NodeList` 集合，而 `getElementsByTagName` 返回 `HTMLCollection` 集合。

从上面的 HTMLCollection 与 NodeList 的区别可以知道，HTMLCollection 是一个动态集合，如果 DOM 树发生变化，HTMLCollection 也会随之变化。而 NodeList 既可以是静态集合也可以是动态集合，这与获取的方式有关，使用 `querySelectorAll` 获取到的是一个静态集合。

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
const ul = document.querySelectorAll('ul')[0]
const liList = document.querySelectorAll('li')

console.log(liList) // NodeList(5) [li, li, li, li, li]
console.log(liList.length) // 5

ul.appendChild(document.createElement('li'))

console.log(liList.length) // 5
liList[liList.length - 1].innerHTML = '🤐'
console.log(liList[liList.length - 1].innerHTML) // 🤐
```

可以看到，使用 `querySelectorAll` 获取 `<li>` 返回的是一个 NodeList 集合。页面 DOM 结构发生改变，其长度不会发生任何变化，而改变内容，则会发生相应的改变。

使用 `Node.childNodes` 属性获取到的 NodeList 则是一个动态集合。

```js
const ul = document.querySelectorAll('ul')[0]

console.log(ul.childNodes.length) // 11
ul.appendChild(document.createElement('li'))
console.log(ul.childNodes.length) // 12
```

HTMLCollection 示例：

```js
const ul = document.querySelectorAll('ul')[0]
const liList = document.getElementsByTagName('li')

console.log(liList) // HTMLCollection(5) [li, li, li, li, li]
console.log(liList.length) // 5

ul.appendChild(document.createElement('li'))

console.log(liList.length) // 6
```

可以看到，使用 `getElementsByTagName` 获取 `<li>` 返回的是一个 HTMLCollection 集合。页面 DOM 结构发生改变，将动态获取其长度。

## 哪种类型更好

通常，拥有一个保持不变的列表是一件好事。但有时你可能需要一个自动更新以反映当前 UI 的列表。

了解选择器方法是返回静态集合还是动态集合也有助于防止意外的副作用和错误。
