# 使 abbr 元素适用于触摸屏、键盘和鼠标

`<abbr>` 元素用于表示和定义首字母缩略词，当你传递一个 `title` 属性时，它将创建一个工具提示。

```html
<abbr title="我是超人">I am Superman</abbr>
```

如果用户将鼠标悬停在缩写词上，则完整定义将显示在本机浏览器工具提示中。

```html
<abbr title="Cascading Style Sheets">CSS</abbr>
```

如果用户将鼠标悬停在缩写词上，则完整定义将显示在本机浏览器工具提示中。

![abbr 提示](https://upload-images.jianshu.io/upload_images/18281896-425c0d50a7114037.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

> **注意**：`<abbr>` 元素不同浏览器的默认样式有些不同。在 Chrome 和 Firefox 中，它将带有下划线，并且在悬停时将带有 `title` 传递的值的工具提示。如果您在 Safari 上打开此页面，则不会出现下划线。此外，仅当您具有 `title` 属性时才显示下划线。

定义术语时，你还可以与 `<dfn>` 混合使用

```html
<dfn> <abbr title="Today I learned">TIL</abbr> something awesome! </dfn>
```

## `abbr` 元素的问题

只要用户与定点设备交互，`<abbr>` 元素就可以在桌面设备上正常工作。但是，对于通过键盘或触摸屏设备与页面交互的用户来说，它失败了。这是因为两件事：

- `<abbr>` 元素无法聚焦，例如，键盘用户按下 `tab` 键。
- 移动浏览器不会在用户的任何交互上显示 **Tooltip**。

而且还存在着一个问题是，原本的 **Tooltip** 会在大约 2 秒的时间才会出现，这与 Web 上的大多数其他交互相比非常慢。

## 修复 `abbr` 元素

可以通过修改 HTML 和添加一些自定义样式来修复这些问题，从而以更有效的方式重新实现本机浏览器工具提示。

### 启用焦点

我们需要做的第一件事是让 `<abbr>` 元素成为焦点，让键盘用户能够与之交互。通过向元素添加 `tabindex` 属性来实现这一点，指示在顺序键盘导航中是可聚焦的，然后在聚焦时触发我们的非缩写内容。

```html
<abbr title="我是超人" tabindex="0">I am Superman</abbr>
```

`tabindex` 值为 `0` 时，元素将按默认的焦点顺序放置，该顺序由元素在 HTML 文档中的位置决定。

### 显示 `title` 元素

接下来，我们需要一种向用户显示 `title` 元素内容的方法。这可以通过使用 `attr()` 函数来实现，该函数允许我们检索并使用任何元素属性的内容作为 CSS 值。

例如，我们可以获取 `<abbr>` 的标题，并在 `::after` 伪元素中的元素之后立即显示它。

```css
abbr[title]::after {
  content: attr(title);
}
```

## 在鼠标悬停、键盘焦点或手指点击时显示自定义工具提示

尽管我们可以把它放在显示 `<abbr>` 元素的 `title` 之后，但我们的目标是重新创建一个 **Tooltip**。因此，我们可以先等待所需的用户交互，而不是在默认情况下简单地显示 `title`。

为了在我们想要的三种场景（鼠标悬停、键盘焦点或手指点击）中显示自定义工具提示，我们需要将伪元素的显示与 `:hover` 和 `:focus` 伪类结合起来。

```css
abbr[title]:hover::after,
abbr[title]:focus::after {
  content: attr(title);
}
```

`:focus` 伪类负责用户通过键盘将元素置于焦点。通过我们添加的 `tabindex` 属性，用户现在可以这样做了。

### 设置工具提示的样式

由于跨浏览器的差异，我们建议为 `<abbr>` 代码加上自定义样式。这样，您将在浏览器之间拥有一致的外观，且使 **Tooltip** 显示为 **Tooltip**，而不仅仅是元素旁边的文本。

```css
abbr[title] {
  position: relative;
  /* 确保跨浏览器的样式一致 */
  text-decoration: underline dotted;
}

abbr[title]:hover::after,
abbr[title]:focus::after {
  content: attr(title);
  position: absolute;
  left: 0;
  bottom: -30px;
  width: auto;
  white-space: nowrap;
  background-color: #1e1e1e;
  color: #fff;
  border-radius: 3px;
  box-shadow: 1px 1px 5px 0 rgba(0, 0, 0, 0.4);
  font-size: 14px;
  padding: 3px 5px;
}
```

最终效果如下：

![更改后的样式](https://upload-images.jianshu.io/upload_images/18281896-a80548e76d99737f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

> 👉 [查看效果](https://codepen.io/lio-zero/pen/BaRjpK)

## 最后

对比原来的效果，明显更加好看了一些，如果你想要更加好看的工具提示，可以继续更改其外观，或者使用现有的一些提示工具，如 Bootstrap 的[工具提示](https://getbootstrap.com/docs/4.5/components/tooltips/)组件。
