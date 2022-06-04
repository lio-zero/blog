# 最小化 DOM 访问

DOM 操作，包括访问 DOM，通常速度较慢。这通常不是问题，除非您必须执行许多 DOM 操作，并且 JavaScript 应用程序的性能开始受到影响。提高性能的一个非常快速的技巧是，如果您计划多次访问 DOM 元素或其值，则将其存储在局部变量中。

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
