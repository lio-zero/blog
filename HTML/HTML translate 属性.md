# HTML translate 属性

HTML 标准规定，默认情况下，所有 HTML 元素都处于**启用翻译的状态**，这意味着它们的文本内容和一些属性（包括 `alt` 和 `placeholder`）**在页面本地化时将被翻译**。

HTML5 新增了一个 `translate` 属性，通过将 `translate` 属性设置为 `no`，网页可以将特定 HTML 元素的这种状态更改为 [`no-translate`](https://html.spec.whatwg.org/multipage/dom.html#the-translate-attribute)。

> [WHATWG](https://blog.whatwg.org/weekly-translate-attribute) 博客写道：默认情况下，所有内容都可以翻译。你可以通过将 `translate` 属性设置为 `"no"` 值来覆盖它。这可用于名称、计算机代码、仅在给定语言中有意义的表达式等。

`translate="no"` 属性指示在线翻译服务以网页的原始语言保留单词或短语。

例如，在一个用德语编写的网页上，句子 “Mein Kampf wurde 1923 veröffentlicht” 被谷歌翻译成 “我的战斗发表于 1923 年”。在这种情况下，将书名包含在 `translate="no"` 元素中将产生最佳翻译（“Mein Kampf 于 1923 年出版”）：

```html
<p>
  <em translate="no" class="notranslate">Mein Kampf</em>
  wurde 1923 veröffentlicht.
</p>
```

**Mein Kampf** 将不会被翻译。

[W3C 使用 HTML 的 `translate` 属性](https://www.w3.org/International/questions/qa-translate-flag)指南也提供了多个使用示例。
