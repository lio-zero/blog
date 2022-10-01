# 仅 CSS 添加暗黑模式

我们只需要添加以下 CSS 即可支持暗模式：

```css
html[theme='dark-mode'] {
  filter: invert(1) hue-rotate(180deg);
}
```

CSS `filter` 属性将模糊或颜色偏移等图形效果应用于元素。滤镜通常用于调整图像，背景和边框的渲染。

对于暗黑模式，将使用两个 `filter` 的两个函数：`invert` 和 `hue-rotate`。

- [`invert`](https://developer.mozilla.org/en-US/docs/Web/CSS/filter-function/invert) — 反转元素和图片的颜色。在 100% 的情况下会完全反转，即黑色变为白色，白色变为黑色，#222 -> #ddd。
- [`hue-rotate`](https://developer.mozilla.org/en-US/docs/Web/CSS/filter-function/hue-rotate) — 帮助我们处理所有其他非黑色和白色的颜色。 将色相旋转 180 度，我们确保应用的颜色主题不会改变，而只是减弱其颜色。

注意，它还会反转应用中的所有图像和视频。如果不想给图片、视频应用暗黑模式，可以添加：

```css
img,
video {
  filter: invert(1) hue-rotate(180deg);
}
```

它将对所有图像和视频添加相同的规则，以逆转效果。

## 更多资料

如果您想要获取系统是否处于暗黑模式下有两种方式，可以查看[检测暗模式](https://github.com/lio-zero/blog/blob/master/JavaScript/%E6%A3%80%E6%B5%8B%E6%9A%97%E6%A8%A1%E5%BC%8F.md)。

推荐一些关于暗模式的文章：

- [A Complete Guide to Dark Mode on the Web](https://css-tricks.com/a-complete-guide-to-dark-mode-on-the-web/)
- [Dark Mode in CSS](https://css-tricks.com/dark-modes-with-css/)
- [A Dark Mode Toggle with React and ThemeProvider](https://css-tricks.com/a-dark-mode-toggle-with-react-and-themeprovider/)
- [Dark Mode Favicons](https://css-tricks.com/dark-mode-favicons/)
- [Dark mode and variable fonts](https://css-tricks.com/dark-mode-and-variable-fonts/)
- [The Quest for the Perfect Dark Mode](https://www.joshwcomeau.com/react/dark-mode/)
- [Dark mode in 5 minutes, with inverted lightness variables](https://lea.verou.me/2021/03/inverted-lightness-variables/)
- [How to Implement Dark Background Mode in CSS](https://codehandbook.org/implement-dark-background-mode-css/)
