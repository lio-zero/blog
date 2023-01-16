# alt 和 title 的区别

`alt` 和 `title` 是 HTML 中用来描述图像和其他元素的两个属性。

它们有以下区别：

- `alt`（alternative text）属性是当元素不能正常呈现时用作元素内容的替代文本。[`<img>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/img) 是使用 `alt` 属性的最常用标签。当图像无法显示时，浏览器将在其位置显示 `alt` 文本，以便用户了解包含图像的含义。还可以被屏幕阅读器读取，帮助残疾人理解图像的意义。
- `title` 属性用来为元素提供附加信息，当鼠标悬停在元素上时，`title` 文本会显示出来。

**注意**，浏览器并非总是会显示图像。当有下列情况时，`alt` 属性可以为图像提供替代的信息：

- 非可视化浏览器（Non-visual browsers）（比如有视力障碍的人使用的音频浏览器）
- 用户选择不显示图像（比如为了节省带宽，或出于隐私等考虑不加载包括图片在内的第三方资源文件）
- 图像文件无效，或是使用了[不支持的格式](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/img#Supported_image_formats)
- 浏览器禁用图像等

## 良好做法

始终为 `<img>` 标签使用 `alt` 属性，以提供一些有用的信息。

```html
<!-- × -->
<img src="wenhao.jpg" alt="图片" />

<!-- √ -->
<img src="wenhao.jpg" alt="满脸问号的男人" />
```

谷歌和其他搜索引擎无法读取图像，但可以看到 `alt` 文本。设置 `alt` 属性是 SEO（搜索引擎优化）的一个良好实践。

通常不会为 `<img>` 设置 `title` 属性，除非它确实提供了有关图像的更多信息。但有人可能会争辩说，如果你必须用标题文本来解释你的图片，也许有更好的图片可以使用。

所以，我们可以总结一下：

- `alt` 属性是必须的，而 `title` 属性是可选的。
- `alt` 是为了在图像加载不出来或不能访问时，给用户补充语义上的信息，而 `title` 是鼠标悬停时的额外说明。

## 技巧

这里有两个选项可以确保在 `img` 标签上始终包含 `alt` 属性。

使用 CSS 为任何缺少或空白 `alt` 属性的 `img` 提供红色轮廓：

```css
img:not([alt]),
img[alt=''] {
  outline: 8px solid red;
}
```

如果你使用的是 VS Code，则可以安装 [webhint](https://marketplace.visualstudio.com/items?itemName=webhint.vscode-webhint) 扩展。

当你将鼠标悬停在元素上时，它将自动检测问题并显示详细信息。

![使用 webhint 检测缺少的备用文本](https://upload-images.jianshu.io/upload_images/18281896-dec384ace4201f38.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
