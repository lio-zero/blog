# Clipboard — 剪贴板操作

<!-- https://davidwalsh.name/navigator-clipboard-api -->

四种实现剪贴板的操作：

- 原生 `document.execCommand()` 方法
- 异步的 Clipboard API
- `copy` 事件和 `paste` 事件
- 第三方库 [clipboard.js](https://github.com/zenorocha/clipboard.js)

前三个的详细内容可以查阅阮一峰老师的[剪贴板操作 Clipboard API 教程](http://www.ruanyifeng.com/blog/2021/01/clipboard-api.html)。我们来看看第四种使用 clipboard.js 库简化原生操作。

## 第三方库 Clipboard.js

首先，在我们的项目中使用 NPM 引入 clipboard：

```js
npm i clipboard
```

或者使用 CDN：

```html
<script src="https://cdn.jsdelivr.net/npm/clipboard@2.0.10/dist/clipboard.min.js"></script>
```

[Clipboard.js](https://github.com/zenorocha/clipboard.js) 的文档已经很全面了，以下是一些简单的实例。

### 复制文本

通过 `data-clipboard-target` 绑定相对应的 id 进行匹配：

```html
<!-- Target -->
<input id="foo" value="https://github.com/zenorocha/clipboard.js.git" />

<!-- Trigger -->
<button class="btn" data-clipboard-target="#foo">
  <img src="assets/clippy.svg" alt="Copy to clipboard" />
</button>
```

更简便的方法是，使用 `data-clipboard-text` 属性设置您想包含的内容即可。

```html
<button class="btn" data-clipboard-text="Copy to Clipboard"></button>
```

### 剪切文本

定义 `data-clipboard-action` 属性，以指定是要 `copy` 还是 `cut` 内容。

```html
<button class="btn" data-clipboard-action="cut" data-clipboard-target="#foo">
  Cut to clipboard
</button>
```

> **注意**：`cut` 操作仅适用于 `<input>` 或 `<textarea>` 元素。

### 事件

`success` 和 `error` 让您监听和实现您的自定义逻辑。

```js
const clipboard = new ClipboardJS('.btn')

clipboard.on('success', (e) => {
  console.info('Action:', e.action)
  console.info('Text:', e.text)
  console.info('Trigger:', e.trigger)

  e.clearSelection()
})

clipboard.on('error', (e) => {
  console.error('Action:', e.action)
  console.error('Trigger:', e.trigger)
})
```

还有一些[高级选项](https://github.com/zenorocha/clipboard.js#advanced-options)，详细见文档。

## 示例：添加著作权

我们在很多知识分享平台都可以看到，当我们复制一段超过一定字数的内容时，会在您复制的内容最后追加上一段标注：

```txt
作者：xxx
链接：xxx
来源：xxx
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。`
```

我们使用 `copy` 事件来实现（这里不对复制多少字数做判断）：

```js
document.addEventListener('copy', (e) => {
  const selection = document.getSelection()
  const ct = `作者：lio
链接：https://github.com/lio-zero
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。`
  e.clipboardData.setData('text/plain', selection.toString() + '\n' + ct)
  e.preventDefault()
})
```

复制这段代码到控制台，试着随意复制一段文本到其他地方。

尝试着，使用其他以上所讲述的任何方法，来修改这个示例，以加深印象。

## 更多资料

- [How do I copy to the clipboard in JavaScript?](https://stackoverflow.com/questions/400212/how-do-i-copy-to-the-clipboard-in-javascript)
- [Unblocking clipboard access](https://web.dev/async-clipboard/)
- [Interact with the clipboard](https://developer.mozilla.org/en-US/docs/Mozilla/Add-ons/WebExtensions/Interact_with_the_clipboard)
- [Multi-MIME Type Copying with the Async Clipboard API](https://blog.tomayac.com/2020/03/20/multi-mime-type-copying-with-the-async-clipboard-api/)
- [利用剪切板 JS API 优化输入框的粘贴体验](https://www.zhangxinxu.com/wordpress/2018/09/js-clipboard-api-paste-input/)
- [js+flash(as3)实现复制文字内容到剪切板](https://www.zhangxinxu.com/wordpress/2010/08/jsflashcs3%e5%ae%9e%e7%8e%b0%e5%a4%8d%e5%88%b6%e6%96%87%e5%ad%97%e5%86%85%e5%ae%b9%e5%88%b0%e5%89%aa%e5%88%87%e6%9d%bf/)
