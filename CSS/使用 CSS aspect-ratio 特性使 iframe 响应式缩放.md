# 使用 CSS aspect-ratio 特性使 iframe 响应式缩放

## iframe 响应式问题

与 `img` 和原生 HTML5 `video` 元素不同，`iframe` 默认情况下不会响应式缩放。

```css
/**
 * 这行不通
 */
iframe {
  max-width: 100%;
  height: auto;
}
```

## 解决此问题的旧方法

从历史上看，要使 `iframe` 具有响应性，需要将 `iframe` 包装在 `div` 容器中。

```html
<div class="responsive-iframe">
  <iframe
    src="/path/to/file"
    width="640"
    height="360"
    frameborder="0"
    allow="autoplay; fullscreen; picture-in-picture"
    allowfullscreen
  >
  </iframe>
</div>
```

然后，您可以使用 CSS

- 将 `iframe` 放在 `div` 的左上角。
- 使其达到 `div` 元素 `height` 和 `width` 的 `100%`。
- 在 `div` 顶部添加等于 `iframe` 纵横比的 `padinng`（对于高清视频，为 `56.25%` 或 `9/16*100`）。

```css
.responsive-iframe {
  max-width: 100%;
  padding-top: 56.25%;
  position: relative;
  width: 100%;
}

.responsive-iframe iframe {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}
```

如您所见，这是令人讨厌和复杂的。

有一个 [FluidVids.js](https://github.com/toddmotto/fluidvids) 库，帮助我们减少这种操作。

但是现在，有一个更好的方法！

## CSS `aspect-ratio` 属性

[CSS `aspect-ratio` 属性](https://developer.mozilla.org/zh-CN/docs/Web/CSS/@media/aspect-ratio)告诉浏览器，在向上或向下缩放某个元素时，要在该元素上保留特定的纵横比。

我们可以使用几行 CSS 直接针对 `iframe` 元素，以在响应性布局中保留它们的尺寸，而不需要包装器元素。

将 `iframe` 的 `height` 和 `width` 设置为 `100%`。然后将 `aspect-ratio` 属性的值指定为 `width / height`。对于高清视频，我们将使用 `16/9`。

```css
iframe {
  aspect-ratio: 16 / 9;
  height: 100%;
  width: 100%;
}
```

现在，我们可以 `iframe` 按原样包含元素，它们会随着布局放大或缩小。

```html
<div class="responsive-iframe">
  <iframe
    src="/path/to/file"
    width="640"
    height="360"
    frameborder="0"
    allow="autoplay; fullscreen; picture-in-picture"
    allowfullscreen
  >
  </iframe>
</div>
```

快速查看 `aspect-ratio` 兼容情况：

![aspect-ratio](https://upload-images.jianshu.io/upload_images/18281896-b9e25e8a37d2ea37.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
