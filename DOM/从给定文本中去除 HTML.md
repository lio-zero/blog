# 从给定文本中去除 HTML

## 从伪元素获取文本内容（不推荐）

```js
const stripHTML = (html) => {
  // 创建新元素
  const ele = document.createElement('div')

  // 设置它的 HTML
  ele.innerHTML = html

  // 只返回文本
  return ele.textContent || ''
}
```

不建议使用这种方法，因为如果 `html` 包含特殊的标记，例如 `<script>`，则可能会导致安全问题。但是，我们可以通过将 `div` 标签替换为 `textarea` 来防止执行 html：

```js
const stripHTML = function (html) {
  const ele = document.createElement('textarea')
  ele.innerHTML = html
  return ele.textContent || ''
}
```

## 使用 DOMParser

[**`DOMParser`**](https://developer.mozilla.org/zh-CN/docs/Web/API/DOMParser) 可以将存储在字符串中的 XML 或 HTML 源代码解析为一个 DOM `Document`。

```js
const stripHTML = function (html) {
  const doc = new DOMParser().parseFromString(html, 'text/html')
  return doc.body.textContent || ''
}
```

## 使用 template

`<template>` 标签包含一个不会立即渲染的 HTML 内容。但是，IE 11 等旧浏览器不支持这种功能。

```js
const stripHTML = (html) => {
  const ele = document.createElement('template')
  ele.innerHTML = html
  return ele.content.textContent || ''
}
```
