# 如何在用户停止输入 JavaScript 后执行函数

我们开发中可能经常会遇到这样的问题，我们想要制作一个输入一个关键字，实时显示需要搜索的内容，但这会使你在每次触发事件时，都会执行你想要执行的筛选函数。但我们只想在用户停止在文本框中键入文本后在执行搜索或过滤某些结果。这时，我们应该怎么做呢？

这就要使用到今天要讲的防抖了，它的作用是：无论触发多少次事件，动作只会执行一次，也就是在一定程度的不活动之后执行函数。它的用处很多，比如上面这个问题。

下面我们完成上面的示例来加深印象。

## 示例：过滤文章

假设我们的文档中有一个要筛选的文章的列表。我们不希望不断地执行 `filter` 函数，但只能在用户 “完成” 键入之后执行。因为我们不知道用户什么时候完成了，所以我们可以猜测，如果用户暂停一秒钟，我们就可以安全地执行该函数（这可能因您的用例而异）。

最终，我们希望效果如下：

![过滤文章](https://upload-images.jianshu.io/upload_images/18281896-6869efb20e77f963.gif?imageMogr2/auto-orient/strip)

HTML 结构如下：

```html
<input type="text" class="article-filter" />
<ul class="article-list">
  <li>HTML</li>
  <li>JavaScript</li>
  <li>CSS</li>
  <li>Vue</li>
  <li>React</li>
  <li>Webpack</li>
</ul>
```

## 如何工作？

防抖背后的思路是，我们有一个全局 JavaScript 变量，该变量将保存对 `setTimeout` 的引用，用于清除所有未完成的计时器。

然后，我们的输入上有一个 `keyup` 事件监听器。当触发此事件时，我们清除全局计时器（如果它正在运行）。这意味着计时器只有在指定的超时之前没有被清除时才会触发，换句话说，只有在超时之前没有 `keyup` 事件时才会触发！

```js
let timer
const input = document.querySelector('.article-filter')
input.addEventListener('keyup', function (e) {
  // 清除所有未完成的计时器
  clearTimeout(timer)
  // 设置可能清除或不清除的新计时器
  timer = setTimeout(() => {
    // ...
  }, 1000)
})
```

## 完整示例

现在，我们大致的结构已经出来了，接着我们将遍历 `.article-list` 元素的所有 `children`，并相应地更新 `style.display` 属性。

```js
let timer
const input = document.querySelector('.article-filter')
input.addEventListener('keyup', function (e) {
  // 清除所有未完成的计时器
  clearTimeout(timer)
  // 设置可能清除或不清除的新计时器
  timer = setTimeout(() => {
    const items = document.querySelector('.article-list').children
    for (let item of items) {
      item.style.display = item.textContent.includes(e.target.value)
        ? 'list-item'
        : 'none'
    }
  }, 1000)
})
```

> **注意**：以上示例没有对大小写进行区分。
