# 指示缺少 alt 属性的 img 元素

以下 CSS 为任何缺少 `alt` 属性或 `alt` 属性为空的 `img` 提供红色轮廓：

```css
img:not([alt]),
img[alt=''] {
  outline: 8px solid red;
}
```

如果您使用的是 Visual Studio Code，则可以安装 [webhint 扩展](https://marketplace.visualstudio.com/items?itemName=webhint.vscode-webhint)。当您将鼠标悬停在元素上时，它将自动检测问题并显示详细信息。

![webhint image](https://upload-images.jianshu.io/upload_images/18281896-cbee5653f019dc2e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
