# stopImmediatePropagation 与 stopPropagation

`stopImmediatePropagation()` 方法可以防止事件像 `stopPropagation()` 方法一样冒泡到父元素。但是，它会阻止调用相同事件的其他监听器。

假设我们将处理同一事件的不同侦听器附加到相同元素。当事件发生时，监听器的执行顺序与添加的顺序相同。

如果在给定的监听器中调用 `stopImmediatePropagation()` 方法，则不会调用其余的监听器。

在下面的示例代码中，有 3 个监听器处理由 `button` 表示的按钮的单击事件。

```js
button.addEventListener('click', function () {
  console.log('foo')
})

button.addEventListener('click', function (e) {
  console.log('bar')
  e.stopImmediatePropagation()
})

button.addEventListener('click', function () {
  console.log('baz')
})
```

单击按钮将在控制台中打印 `foo` 和 `bar`。我们不会看到 `baz`，因为最后一个监听器没有被调用。
