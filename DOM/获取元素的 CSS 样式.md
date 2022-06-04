# 获取元素的 CSS 样式

我们可以使用 `getComputedStyle` 方法获取 `ele` 元素的所有 CSS 样式：

```js
const styles = window.getComputedStyle(ele, null)
```

访问特定样式的值：

```js
// 获取背景颜色
const bgColor = styles.backgroundColor
```

对于供应商前缀以连字符（`-`）开头的样式，我们可以通过传递样式来获取样式值：

```js
const textSizeAdjust = styles['-webkit-text-size-adjust']
```

`getPropertyValue` 方法产生相同的结果：

```js
const bgColor = styles.getPropertyValue('background-color')

// 或者把参数转成 camelCase 格式：
const bgColor = styles.getPropertyValue('backgroundColor')
```
