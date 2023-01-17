# 最小化 DOM 访问

DOM 操作，包括访问 DOM，通常速度较慢。这通常不是问题，除非你必须执行许多 DOM 操作，并且 JavaScript 应用程序的性能开始受到影响。提高性能的一个非常快速的技巧是，如果你计划多次访问 DOM 元素或其值，则将其存储在局部变量中。

```js
// 这很慢，它会多次访问 DOM 元素
document.querySelector('.ele').classList.add('my-class')
document.querySelector('.ele').textContent = 'hello'
document.querySelector('.ele').hidden = false

// 这会更快，因为它将 DOM 元素存储在一个变量中
const myElement = document.querySelector('.ele')
myElement.classList.add('my-class')
myElement.textContent = 'hello'
myElement.hidden = false
```

**注意**，虽然这个技巧可能会派上用场，但它附带了一条警告：如果以后删除 DOM 元素，并且仍然将其存储在变量中，则应将该变量设置为 `null`，以避免潜在的内存泄漏。

## 对 DOM 操作的一些建议

- 尽量减少对 DOM 的查询。使用变量缓存 DOM 元素可以减少对 DOM 的重复查询。例如，如果你需要在多个地方访问同一个元素，可以将其存储在变量中，而不是每次都重新查询。
- 使用事件委托可以减少对 DOM 的访问。事件委托是将事件监听器添加到父元素上，而不是每个子元素上。这样可以减少对 DOM 的查询和事件监听器的数量。
- 尽量减少对 DOM 的修改。修改 DOM 会导致浏览器重新渲染页面，这可能会影响性能。如果可能的话，应该尽量减少对 DOM 的修改次数，并在一次性修改多个元素。
- 使用 `requestAnimationFrame` 函数来优化动画性能。这个函数会在下一次浏览器重绘时调用回调函数，这样可以在动画运行时减少对 DOM 的访问。

最小化 DOM 访问可以提高网页性能，帮助网页更快地加载和更流畅地运行。
