# 使用 JavaScript 获取、设置和删除属性

## `Element.*Attribute()`方法

您可以使用 `Element.getAttribute()`、`Element.setAttribute()`、`Element.removeAttribute()` 和 `Element.hasAttribute()` 方法分别获取、设置、删除和检查元素上是否存在属性（包括 `data-*` 属性）。

如果元素上不存在属性，则 `Element.getAttribute()` 方法返回 `null`。

```js
let elem = document.querySelector('#lunch')

// 获取 [data-message] 属性的值
let message = elem.getAttribute('data-message')

// 设置 [data-message] 属性的值
elem.setAttribute('data-message', 'Hello World!')

// 移除 [data-id] 属性
elem.removeAttribute('data-id')

// 检查一个元素是否有 [data-name] 属性
if (elem.hasAttribute('data-name')) {
  console.log('添加一个名字!')
}
```

> [获取、设置和删除 data-* 属性](https://github.com/lio-zero/blog/blob/master/DOM/%E8%8E%B7%E5%8F%96%E3%80%81%E8%AE%BE%E7%BD%AE%E5%92%8C%E5%88%A0%E9%99%A4%20data-%20%E5%B1%9E%E6%80%A7.md)

## 元素属性

HTML 元素有许多可以直接访问的属性。

其中一些是只读的，这意味着您可以获取它们的值，但不能设置它。其他的可用于读取和设置值。

> 详细内容你可以在 [Mozilla 开发者网络上找到完整的列表](https://developer.mozilla.org/en-US/docs/Web/API/element)。

```js
let elem = document.querySelector('#main')

// 获取元素的 ID
let id = elem.id // "main"

// 设置元素的 ID
elem.id = 'secondary'

// 获取元素的 parentNode
// 该属性是只读的
let parent = elem.parentNode
```

## attributes 和 properties 有什么区别？

在 JavaScript 中，元素具有 attributes 和 properties。这些术语通常可以互换使用，但它们实际上是两个不同的东西。

`attributes` 是在 DOM 中呈现时的初始状态。`propertie` 是当前状态。

在大多数情况下，它俩会自动保持同步。例如，当您使用 `Element.setAttribute()` 更新 ID 属性时，`id` 属性也会更新。

```html
<p>Hello</p>
```

```js
let p = document.querySelector('p')

// 更新 ID
p.setAttribute('id', 'first-paragraph')

// 它们都返回 "first-paragraph"
let id1 = p.getAttribute('id')
let id2 = p.id
```

但是，用户可更改的表单属性（尤其是 `value`、`checked` 和 `selected`）不会自动同步。

```html
<label for="greeting">Greeting</label> <input type="text" id="greeting" />
```

```js
let greeting = document.querySelector('#greeting')

// 更新值
greeting.setAttribute('value', 'Hello there!')

// 如果您没有对该字段进行任何更新，那么它们都将返回 Hello there!
// 如果你已经更新了字段，val1 返回任何在字段中输入的内容
let val1 = greeting.value
let val2 = greeting.getAttribute('value')
```

如果您尝试直接更新 `value` 属性，这会更新 UI。

```js
greeting.value = 'Hello there!'
```

这允许您根据是否要覆盖用户更新来选择不同的方法。

如果您想更新一个字段，但只是在用户没有做任何更改的情况下，请使用 `Element.setAttribute()`。如果您想要覆盖他们所做的任何事情，请使用 `value` 属性。
