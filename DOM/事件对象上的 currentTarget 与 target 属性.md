# 事件对象上的 currentTarget 与 target 属性

`currentTarget` 和 `target` 是监听特定事件时事件对象的属性，例如：

```js
element.addEventListener('click', (e) => {
  console.log(e.currentTarget)
  console.log(e.target)
})
```

## 区别

`currentTarget` 是事件绑定到的元素。在上面的示例代码中，`e.currentTarget` 是 `element`。

在 `click` 事件的情况下，`target` 是用户单击的元素。它可以是原始元素或其任何子元素，具体取决于用户单击的位置。

## 用例

下面是一个用例，演示了这两个属性的用法。

假设我们在屏幕中央显示了一个模态框。模态框被放置在覆盖层内，覆盖层采用屏幕的全尺寸。

结构非常简单，如下所示：

```html
<!-- 覆盖层 -->
<div id="overlay">
  <!-- 模态框内容 -->
  <div id="modal">...</div>
</div>
```

用户在单击覆盖层时关闭模态框是一种常见的体验。

有两种方法可以做到这一点，但首先，我们需要获取模态框和覆盖层的元素：

```js
const overlay = document.getElementById('overlay')
const modal = document.getElementById('modal')
```

**第一种方法**：当用户单击覆盖层时，我们关闭模态框：

```js
overlay.addEventListener('click', () => {
  console.log('关闭模态框')
})
```

**如果用户在模态框内单击会发生什么？**我们不希望事件冒泡到父元素（即覆盖层），因此调用 `stopPropagation` 方法：

```js
modal.addEventListener('click', (e) => {
  e.stopPropagation()
})
```

`stopPropagation()` 方法用于阻止捕获和冒泡阶段中当前事件在 DOM 中的进一步传播。示例中，我们阻止模态框的点击事件冒泡到父元素。

> **Tips**：了解 [JavaScript 事件传播机制](https://github.com/lio-zero/blog/blob/main/DOM/JavaScript%20%E4%BA%8B%E4%BB%B6%E4%BC%A0%E6%92%AD%E6%9C%BA%E5%88%B6.md)可以帮助您更好的理解上面的做法。

**第二种方法**：为了确保用户单击覆盖区域而不是模态内部，我们可以简单地检查 `currentTarget` 和 `target` 属性是否引用相同的元素：

```js
overlay.addEventListener('click', (e) => {
  if (e.currentTarget === e.target) {
    console.log('关闭模态框')
  }
})
```

第二种方法比第一种方法简单得多，并且不需要处理模态框的 `click` 事件。
