# 使用 mark 元素突出显示文本

HTML5 中的语义化 `<mark>` 元素可用于突出显示文本。它可以将文本的背景颜色设置为黄色，表示该文本为重点。

突出显示搜索结果中的关键字是使用 `<mark>` 元素的一个流行示例。

```html
Smooth <mark>scrolling</mark> with pure CSS
```

这样，文本 **scrolling** 会被突出显示出来。

默认情况下，`mark` 元素会使用浏览器默认的黄色背景颜色，但是你可以使用 CSS 更改你想要的效果。

```css
mark {
  background: plum;
  color: white;
  font-size: 1.2em;
}
```
