# 使用 JavaScript 从多个元素中添加和删除 CSS 类

假设我们有以下结构：

```html
<p class="color-purple">Hello</p>
<p>there!</p>
<p class="color-purple">How</p>
<p>are</p>
<p class="color-purple">you?</p>
```

样式如下：

```css
.color-plum {
  color: plum;
}

.color-purple {
  color: rebeccapurple;
}
```

我们想要在多个元素上添加或删除某个类，下面我们来看看如何使用 JavaScript 从多个元素中添加和删除 CSS 类。

## 获取所有元素

想要获取所有的元素，可以使用 `document.querySelectorAll()` 方法。它可以传入任何有效的选择器，并返回一个 NodeList 匹配元素。

```js
let p = document.querySelectorAll('p')
let purple = document.querySelectorAll('p.color-purple')
```

## 添加和删除类

您可以使用 `Element.classList` API 从元素中添加和删除类。

它提供了一些可用于添加、删除、切换和检查元素上的类的方法。我们可以使用 `add()` 方法添加类，以及 `remove()` 方法 删除类。

此方法仅适用于单个元素，而不适用于集合。

```js
let elem = document.querySelector('p.color-purple')

elem.classList.add('color-blue')
elem.classList.remove('color-purple')
```

如果要添加多个类，可以传入多个参数：

```js
elem.classList.add('color-blue', 'text-large')
```

## 循环遍历每个元素

要从多个元素中添加和删除类，我们需要循环遍历从 `document.querySelectorAll()` 方法返回的 NodeList 中的每个类，并使用 `Element.classList` API。

有很多方法可以做到这一点，但最简单的两种方法是 `NodeList.forEach()` 方法和 `for...of` 循环。

- `for...of` 循环

```js
for (let elem of p) {
  elem.classList.add('color-blue')
}

for (let elem of purple) {
  elem.classList.remove('color-purple')
}
```

- `NodeList.forEach()` 方法

```js
p.forEach(function (elem) {
  elem.classList.add('color-blue')
})

purple.forEach(function (elem) {
  elem.classList.remove('color-purple')
})
```

`NodeList.forEach()` 方法也可以直接在 `document.querySelectorAll()` 方法返回的 NodeList 上调用，而无需将其保存到变量中。这有时可能很有用。

```js
document.querySelectorAll('p').forEach(function (elem) {
  elem.classList.add('color-blue')
})
```

## 辅助函数

创建一个辅助函数，帮助我们减少一些重复的操作：

```js
function addClass(elems, ...classes) {
  // 添加 .color-blue 类
  for (let elem of elems) {
    elem.classList.add(...classes)
  }
}

function removeClass(elems, ...classes) {
  for (let elem of elems) {
    elem.classList.remove(...classes)
  }
}
```

## 更多资料

- [HTMLCollection 和 NodeList 的区别](https://github.com/lio-zero/blog/blob/master/DOM/HTMLCollection%20%E5%92%8C%20NodeList%20%E7%9A%84%E5%8C%BA%E5%88%AB.md)
- [切换 HTML 元素的类](https://github.com/lio-zero/blog/blob/master/DOM/%E5%88%87%E6%8D%A2%20HTML%20%E5%85%83%E7%B4%A0%E7%9A%84%E7%B1%BB.md)
