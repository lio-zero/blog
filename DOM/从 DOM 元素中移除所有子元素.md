# 从 DOM 元素中移除所有子元素

给定 DOM 中的一个项目列表，使用 `querySelector()` 获取它，如下所示：

```js
const parent = document.querySelector('.parent-element')
```

要删除其所有子元素，你有几个不同的解决方案。

最快的解决方案是使用 DOM 元素的 `innerHTML` 或 `textContent` 属性来移除所有子元素：

```js
parent.innerHTML = ''
```

另一个解决方案是，创建一个循环，检查 `firstChild` 属性是否存在，然后使用 DOM 元素的 `removeChild()` 方法将其删除：

```js
while (parent.firstChild) {
  parent.removeChild(parent.firstChild)
}
```

当 `parent` 的所有子元素都被移除时，循环结束。

还有一种方法是使用 `Node.remove()`，如果你的浏览器支持这个方法，可以直接调用：

```js
while (parent.firstElementChild) {
  parent.firstElementChild.remove()
}
```

## 更多资料

[从 DOM 中移除一个元素](https://github.com/lio-zero/blog/blob/main/DOM/%E4%BB%8E%20DOM%20%E4%B8%AD%E7%A7%BB%E9%99%A4%E4%B8%80%E4%B8%AA%E5%85%83%E7%B4%A0.md)
