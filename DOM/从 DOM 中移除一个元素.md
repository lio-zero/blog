# 从 DOM 中移除一个元素

- 使用 `Element.parentNode` 获得给定元素的父节点。
- 使用 `Element.removeChild()` 从其父节点中删除给定元素。

```js
const removeElement = (el) => el.parentNode.removeChild(el)

// 从 DOM 中移除 .ele
removeElement(document.querySelector('.ele'))
```
