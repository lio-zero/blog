# DOMContentLoaded 与 load 的区别

当页面中的所有节点都已在 DOM 树中构建时，将触发 `DOMContentLoaded` 事件。当所有资源（如图像和子帧）都已完全加载时，将触发 `load` 事件。

## readyState 属性

IE 8 和旧版本的 IE 不支持 `DOMContentLoaded` 事件。

为了支持旧版本 IE 中的行为，我们可以使用 `readyState` 属性检查文档是否已完全加载：

```js
const ready = function (cb) {
  if (document.readyState === 'loading') {
    //文档仍在加载中
    document.addEventListener('DOMContentLoaded', function (e) {
      cb()
    })
  } else {
    cb() // 文档已完全加载
  }
}
```

`ready` 函数接受一个函数，该函数将在文档准备好时被调用:

```js
ready(function () {
  // 文件准备好后做些什么
  // ...
})
```
