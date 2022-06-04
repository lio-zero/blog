# 切换 HTML 元素的类

使用 `Element.classList` 和 `DOMTokenList.toggle()` 切换元素的指定类。

```js
const toggleClass = (el, className) => el.classList.toggle(className)

toggleClass(document.querySelector('p.hdfp'), 'hdfp')
```
