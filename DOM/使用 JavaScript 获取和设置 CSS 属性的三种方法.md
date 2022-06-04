# 使用 JavaScript 获取和设置 CSS 属性的三种方法

**前提说明**：

JavaScript 使用驼峰式版本的属性与 CSS 中使用的属性一一对应。

例如，CSS 中的 `background-image` 就是 JavaScript 中的 `backgroundImage`。CSS 中的 `font-weight` 属性在 JavaScript 中是 `fontWeight`。

> 详细可查看 [Mozilla 开发者网络提供了可用属性及其 JavaScript 对应项的全面列表](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Properties_Reference)

## `Element.style` 属性

您可以使用 `Element.style` 属性获取和设置元素的内联样式。

`Element.style` 属性是一个只读对象。您可以使用 camelCase 样式名称作为对象的属性来获取和设置其上的单个样式属性 。

```html
<p class="elem" style="background-color: plum; color: white;">Hello World!</p>
```

```js
let elem = document.querySelector('.elem')

// 如果这个样式没有直接在元素上设置为内联样式，它将返回一个空字符串
let bgColor = elem.style.backgroundColor // "plum"
let fontWeight = elem.style.fontWeight // ""

// 设置 background-color 样式属性
elem.style.backgroundColor = 'purple'
```

您还可以使用 `Element.style.csstext` 属性在元素本身上获取和设置整个内联 `style` 属性的字符串表示。

```js
// 获取元素的样式
let styles = sandwich.style.cssText // "background-color: green; color: white;"

// 完全替换元素上的内联样式
sandwich.style.cssText = 'font-size: 2em; font-weight: bold;'

// 添加额外的样式
sandwich.style.cssText += 'color: purple;'
```

## `window.getComputedStyle()` 方法

`window.getComputedStyle()` 方法获取元素的实际计算样式。这会影响浏览器默认样式以及页面上使用的外部样式表。

```js
let elem = document.querySelector('.elem')
let bgColor = window.getComputedStyle(elem).backgroundColor
```

这是只读的，不能用于实际修改元素的样式。

## 向文档添加样式

`Element.style` 属性对于向特定元素添加内联样式很有用。

但是，如果您想为所有匹配特定选择器的元素添加样式怎么办？您可以遍历每个匹配元素并使用 `Element.style` 属性添加样式。

```js
let elems = document.querySelectorAll('.elem')

for (let el of elems) {
  el.style.backgroundColor = 'rebeccapurple'
  el.style.color = 'white'
}
```

或者，您可以通过创建一个 `style` 元素并将其附加到 DOM 中来直接向文档中添加 CSS。

首先，使用 `document.createElement()` 方法创建一个 `style` 元素。然后，使用 `Element.textContent` 属性将您的 CSS 添加到其中。

最后，您可以使用 `Element.append()` 方法将其注入到文档中。我喜欢附加到 `document.head`，但 `document.body` 也可以。

```js
let style = document.createElement('style')
style.textContent = `.elem {
    background-color: rebeccapurple
    color: white;
  }`
document.head.append(style)
```
