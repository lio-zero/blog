# JavaScript 中响应图像的最大宽度和高度

当我们操作砌体布局时，经常需要获取图像的当前宽度和高度，现代浏览器为我们提供了两个属性来方便的获取这些值。

现代浏览器（包括 IE9）为 `<img>` 元素提供 `naturalWidth` 和 `naturalHeight` 属性。这些属性包含图像的实际、未修改的宽度和高度。

```js
const maxWidth = document.getElementById('img').naturalWidth
const maxHeight = document.getElementById('img').naturalHeight
```

`naturalWidth` 和 `naturalHeight` 提供给我们图像的像素值，以下是这两个属性的支持情况：

![naturalWidth 和 naturalHeight 的支持情况](https://upload-images.jianshu.io/upload_images/18281896-1250e8e4c0984d79.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

它们支持几乎所有主流的浏览器。

IE8 或更低版本不支持 `naturalWidth` 和 `naturalHeight` 属性。

如果你需要支持例如 IE8 以下的浏览器，你可以试着创建一个 `image` 对象，来增加对旧浏览器的支持。

```js
// 例如：获取图像的 max-width:100%; 以像素为单位
function getMaxWidth(img) {
  let maxWidth

  // 检查是否支持 naturalWidth
  if (img.naturalWidth !== undefined) {
    maxWidth = img.naturalWidth
    // 不支持，创建回退解决方案
  } else {
    const image = new Image()
    image.src = img.src
    maxWidth = image.width
  }

  // 返回 max-width
  return maxWidth
}
```

**注意**：你应该确保图像必须已加载完成，然后再检查宽度，以确保图像有尺寸：

```js
const hasDimensions = (img) => !!((img.complete && typeof img.naturalWidth !== 'undefined') || img.width)
```
