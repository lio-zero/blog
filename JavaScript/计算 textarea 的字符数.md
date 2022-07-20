# 计算 textarea 的字符数

假设我们有一个 `textarea` 和一个 `div` 元素来显示用户输入了多少字符：

```html
<div>
  <textarea rows="5" maxlength="200" id="message"></textarea>
  <div id="counter">0</div>
</div>
```

这里我们使用 `maxlength` 属性设置用户可以在 `textarea` 中放置的最大字符数。

接下来，我们需要处理 `textarea` 元素值改变时触发的事件：

```js
document.addEventListener('DOMContentLoaded', function () {
  const messageEle = document.getElementById('message')
  const counterEle = document.getElementById('counter')
  // 获取 `maxlength` 属性
  const maxLength = messageEle.getAttribute('maxlength')

  counterEle.innerHTML = `0/${maxLength}`

  messageEle.addEventListener('input', function (e) {
    const target = e.target
    // 计算当前字符数
    const currentLength = target.value.length
    counterEle.innerHTML = `${currentLength}/${maxLength}`
  })
})
```

**注意**，常见的错误是捕获 `keyup` 事件。但它在某些情况下不起作用，例如：

- 用户将文本拖动到文本区域
- 用户在文本区域中单击鼠标右键，然后从关联菜单中选择**粘贴**

[查看效果](https://codepen.io/lio-zero/pen/wveLxox)
