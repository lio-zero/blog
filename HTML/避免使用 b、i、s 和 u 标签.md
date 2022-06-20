# 避免使用 b、i、s 和 u 标签

这些标签通常用于设置样式。建议不要使用它们。相反，使用提供相同外观的语义标签或 CSS 样式。

| 标签  | 推荐方式                        |
| ----- | ------------------------------- |
| `<b>` | `<strong>`                      |
| `<i>` | `<em>`                          |
| `<s>` | `text-decoration: line-through` |
| `<u>` | `text-decoration: underline`    |

## <b>、<i> 与 <strong>、<em> 的区别

默认情况下，`b` 和 `strong` 标签使文本加粗。默认情况下，`i` 和 `em` 标签将文本设置为斜体。

每个浏览器都有自己的默认样式，但会产生类似的外观。以下是它们在流行浏览器中的样式：

[Chrome](https://chromium.googlesource.com/chromium/blink/+/master/Source/core/css/html.css):

```css
strong,
b {
  font-weight: bold;
}

i,
cite,
em,
var,
address,
dfn {
  font-style: italic;
}
```

[Firefox](https://hg.mozilla.org/mozilla-central/file/tip/layout/style/res/html.css):

```css
b,
strong {
  font-weight: bolder;
}

i,
cite,
em,
var,
dfn {
  font-style: italic;
}
```

[Safari](https://trac.webkit.org/browser/trunk/Source/WebCore/css/html.css):

```css
strong,
b {
  font-weight: bold;
}

i,
cite,
em,
var,
address,
dfn {
  font-style: italic;
}
```

尽管它们的外观相似，但 `strong` 和 `em` 标签为所附文本添加了额外的语义，而 `b` 和 `i` 则纯粹是视觉上的。

根据 HTML5 规范，`strong` 和 `em` 标签分别表示重要性和强调。

就可访问性而言，虽然特定的屏幕阅读软件可能会或可能不会以不同的方式发音，但使用 `<strong>` 或 `<em>` 至少可以打开这种可能性，而屏幕阅读软件永远不会以不同的方式发音 `<b>` 或 `<i>` 中的文本。
