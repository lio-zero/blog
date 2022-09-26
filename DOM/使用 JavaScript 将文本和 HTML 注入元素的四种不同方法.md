# 使用 JavaScript 将文本和 HTML 注入元素的四种不同方法

本文将介绍四个用于**获取和设置 DOM 元素中的文本和 HTML**的属性。

## `Element.innerHTML` 属性

您可以使用 `Element.innerHTML` 属性来获取和设置元素内的 HTML 内容作为字符串。

```html
<div class="greeting">
  <p>Hello world!</p>
</div>
```

```js
let greeting = document.querySelector('.greeting')

// 获取 HTML 内容
let html = greeting.innerHTML // "<p>Hello world!</p>"
```

设置 HTML 内容：

```js
// 这取代了已经存在的东西
greeting.innerHTML =
  '这是一段内容，并且还能包含标签，比如：<a href="#">这个链接</a>。'

// 将 HTML 添加到元素现有内容的末尾
greeting.innerHTML += ' 在已经存在的内容之后添加此内容。'

// 将 HTML 添加到元素现有内容的开头
greeting.innerHTML = '我们可以在开头加上这个。' + greeting.innerHTML

// 也可以注入整个元素
greeting.innerHTML += '<p>新段落</p>'
```

## `Element.outerHTML` 属性

您可以使用 `Element.outerHTML` 属性来获取和设置包含元素的 HTML 内容。这与 `Element.innerHTML` 相同，但在获取和更新 HTML 内容时包括元素本身。

```html
<div class="greeting">
  <p>Hello world!</p>
</div>
```

```js
let greeting = document.querySelector('.greeting')

// 获取 HTML 内容
let html = greeting.outerHTML // "<div class="greeting"><p>Hello world!</p></div>"
```

设置 HTML 内容：

```js
// 这将完全取代 <div class="greeting"></div> 元素及其所有内容
greeting.outerHTML =
  '<p class="outro">再见，朋友！<a href="exit.html">点击这里离开。</a>'

// 在元素之后添加 HTML
greeting.outerHTML += ' 在已经存在的内容之后添加此内容。'

// 在元素之前添加 HTML
greeting.outerHTML = '我们可以在开头加上这个。' + greeting.innerHTML
```

## `Node.textContent` 属性

您可以使用 `Node.textContent` 属性来获取和设置元素的文本（并省略标记）作为字符串。

在下面的示例中，您可能会注意到 `Node.textContent` 属性获取**所有**文本内容，包括元素内部的 CSS 属性 `style` 和 `hidden` UI 元素。

使用该属性设置内容时，字符串中包含的任何 HTML 元素都会被 `Node.textContent` 自动编码并按原样渲染。

```html
<div class="greeting">
  <style type="text/css">
    p {
      color: rebeccapurple;
    }
  </style>
  <p hidden>这是不渲染的。</p>
  <p>Hello world!</p>
</div>
```

```js
let greeting = document.querySelector('.greeting')

// 获取文本内容
let text = greeting.textContent // "p {color: rebeccapurple;} 这是不渲染的。Hello world!"
```

设置文本内容：

```js
// 这将完全替换所有的内容，包括任何 HTML 元素
greeting.textContent = '我们可以动态更改内容。'

// 将文本添加到元素现有内容的末尾
greeting.textContent += ' 在已经存在的内容之后添加此内容。'

// 将文本添加到元素现有内容的开头
greeting.textContent = '我们可以在开头加上这个。' + greeting.textContent

// HTML 元素按原样自动编码和渲染
greeting.textContent = '<p>回头见！</p>'
```

## `Element.innerText` 属性

`Element.innerText` 属性获取和设置元素的**渲染文本**（并省略标记）。

与 `Node.textContent` 属性不同，`Element.innerText` 属性仅返回渲染文本，类似于用户在突出显示文本时可以使用光标或键盘选择的内容。

就像 `Node.textContent`，设置内容时字符串中包含的任何 HTML 元素都会自动编码并按原样渲染。

```html
<div class="greeting">
  <style type="text/css">
    p {
      color: rebeccapurple;
    }
  </style>
  <p hidden>这是不渲染的。</p>
  <p>Hello world!</p>
</div>
```

```js
let elem = document.querySelector('.greeting')

// 获取文本内容
let text = elem.innerText // "Hello world!"
```

设置文本内容：

```js
// 这将完全替换那里的内容，包括任何 HTML 元素
elem.innerText = '我们可以动态更改内容。'

// 将文本添加到元素现有内容的末尾
elem.innerText += ' 在已经存在的内容之后添加此内容。'

// 将文本添加到元素现有内容的开头
elem.innerText = '我们可以在开头加上这个。' + elem.innerText

// HTML 元素按原样自动编码和渲染
elem.innerText = '<p>回头见！</p>'
```

## 如何选择？

一般来说，如果您只是修改文本，那么使用 `Node.textContent` 属性是最好、最安全的选择。

对于修改 HTML，`Element.innerHTML` 属性非常有用，但它确实存在一些问题，它会删除事件监听器，并且存在一些安全问题。

> 推荐：[什么是 XSS 攻击？](https://github.com/lio-zero/blog/blob/main/WTF/%E4%BB%80%E4%B9%88%E6%98%AF%20XSS%20%E6%94%BB%E5%87%BB%EF%BC%9F.md)
