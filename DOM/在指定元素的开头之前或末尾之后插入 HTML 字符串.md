# 在指定元素的开头之前或末尾插入 HTML 字符串

使用位置为 `beforebeagin` 的 `Element.insertAdjacentHTML()` 解析 `htmlString` 并将其插入 `el` 开始之前。

```js
const insertBefore = (el, htmlString) =>
  el.insertAdjacentHTML('beforebegin', htmlString)

insertBefore(document.querySelector('#myId'), '<p>before</p>')
// <p>before</p> <div id="myId">...</div>
```

使用位置为 `afterend` 的 `Element.insertAdjacentHTML()` 解析 `htmlString` 并将其插入 `el` 末尾之后。

```js
const insertAfter = (el, htmlString) =>
  el.insertAdjacentHTML('afterend', htmlString)

insertAfter(document.getElementById('myId'), '<p>after</p>')
// <div id="myId">...</div> <p>after</p>
```

<!-- https://flaviocopes.com/insertadjacenthtml/ -->
