# 在指定元素的开头之前插入 HTML 字符串

使用位置为 `beforebeagin` 的 `Element.insertAdjacentHTML()` 解析 `htmlString` 并将其插入 `el` 开始之前。

```js
const insertBefore = (el, htmlString) =>
  el.insertAdjacentHTML('beforebegin', htmlString)

insertBefore(document.querySelector('#myId'), '<p>before</p>')
// <p>before</p> <div id="myId">...</div>
```

## 在其他元素之后或之前插入一个元素

### 插入后

在 `ele` 元素之后插入 `refEle` 元素：

```js
refEle.parentNode.insertBefore(ele, refEle.nextSibling)

// Or
refEle.insertAdjacentElement('afterend', ele)
```

### 之前插入

在 `ele` 元素之前插入 `refEle` 元素：

```js
refEle.parentNode.insertBefore(ele, refEle)

// Or
refEle.insertAdjacentElement('beforebegin', ele)
```
