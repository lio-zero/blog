# keydown、keypress 和 keyup

## 事件顺序

当用户按下一个键或不同的组合键时，按以下顺序触发 `keydown`、`keypress` 和 `keyup`：

- 当用户按键时，会首先触发 `keydown` 事件
- 用户释放按键时最后触发 `keyup` 事件
- 在这之间，`keypress` 事件将被触发

这些事件用于不同的目的。

在 `keydown` 和 `keyup` 事件经常被用来处理物理按键，而 `keypress` 事件被用来处理其正在键入的字符。

`keypress` 会忽略某些按键，诸如 `delete`、`arrows`、`page up`、`page down`、`home`、`end`、`ctrl`、`alt`、`shift` 和`esc` 等。所以，如果我们需要处理这些键，最好使用两种 `keydown` 或 `keyup` 事件。

下面的示例代码监听 `keydown` 事件，并在按下 `ESC` 键时关闭模式：

```js
document.addEventListener('keydown', function (e) {
  if (e.key === 'Escape') {
    // ...
  }
})
```

如果用户按住某个键，则 `keydown` 和 `keypress` 事件会触发多次。

而 `keyup` 仅在用户释放按键时触发一次。
