# CSS Font Loading API

[CSS Font Loading API](https://developer.mozilla.org/en-US/docs/Web/API/CSS_Font_Loading_API) 提供动态加载字体资源的事件和接口。

如果您熟悉 [Web Font Loader](https://github.com/typekit/webfontloader) 库的话，那么就把这个 API 想成是在浏览器中原生实现的。

由一个 `FontFace` 构造函数创建，语法如下：

```js
new FontFace(family, source, descriptors)
```

- `family` — 指定在设置元素样式时可用于匹配此字体的字体系列名称。
- `source` — 字体来源。可以是：
  - 字体文件的 URL。
  - `ArrayBuffer`（或 `ArrayBufferView`）中的二进制字体数据。
- `descriptors` — 可选。作为对象传递的一组可选描述符。

详细 API 请查阅 MDN。

下面通过一个示例来了解它的具体用法。

## 示例

```js
const hasSupport = () => typeof FontFace === 'function'

async function run() {
  try {
    // 定义字体
    const getFont = new FontFace(
      'Bitter',
      'url(https://fonts.gstatic.com/s/bitter/v7/HEpP8tJXlWaYHimsnXgfCOvvDin1pK8aKteLpeZ5c0A.woff2)'
    )

    // 加载字体
    await getFont.load()

    // 将字体添加到文档
    document.fonts.add(getFont)
  } catch (e) {
    console.error('字体加载错误: ', e)
  }
}

run()
```

[查看示例](https://codepen.io/lio-zero/pen/xxjqWYG)

## 更多资料

[Optimizing Web Font Rendering Performance](https://www.igvita.com/2014/01/31/optimizing-web-font-rendering-performance/#font-load-events)
