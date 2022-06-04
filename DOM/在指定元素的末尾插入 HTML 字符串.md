# 在指定元素的末尾插入 HTML 字符串

使用位置为 `afterend` 的 `Element.insertAdjacentHTML()` 解析 `htmlString` 并将其插入 `el` 末尾。

```js
const insertAfter = (el, htmlString) =>
  el.insertAdjacentHTML('afterend', htmlString)

insertAfter(document.getElementById('myId'), '<p>after</p>')
// <div id="myId">...</div> <p>after</p>
```
