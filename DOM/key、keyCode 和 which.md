# key、keyCode 和 which

`key`、`keyCode` 和 `which` 可用于确定按下哪个键。下面是处理文本框的按键事件的示例代码。

它检查用户是否按下键代码为 13 的 **Enter** 键：

```js
textBoxElement.addEventListener('keydown', function (e) {
  if (e.keyCode === 13) {
    // Do something ...
  }
})
```

## 区别

根据 MDN 文档，[`keyCode`](https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent/keyCode) 和 [`which`](https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent/which) 都已弃用，将从 Web 标准中删除。

除此之外，浏览器对这两个属性的支持也有所不同。有些浏览器使用 `keyCode`，其他浏览器使用 `which`。

通常可以看到一些规范化的写法，如下所示：

```js
const key = 'which' in e ? e.which : e.keyCode

// Or
const key = e.which || e.keyCode || 0
```

## 建议

建议使用 [`key`](https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent/key) 属性。上面的示例代码可以重写为：

```js
if (e.key === 'Enter') {
  // 按下回车键
}
```
