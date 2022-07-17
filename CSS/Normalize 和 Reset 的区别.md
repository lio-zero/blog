# Normalize 和 Reset 的区别

每个浏览器都提供一组默认样式，这些样式应用于它呈现的每个网页。默认样式表也称为用户代理样式表。

您可以查看以下浏览器各自所提供的默认样式：

- [Chrome](https://chromium.googlesource.com/chromium/blink/+/master/Source/core/css/html.css)
- [Firefox](https://hg.mozilla.org/mozilla-central/file/tip/layout/style/res/html.css)
- [Safari](https://trac.webkit.org/browser/trunk/Source/WebCore/css/html.css)

由于默认样式不相同，因此每个浏览器上的网页将具有不同的外观。Normalize 和 Reset CSS 都旨在解决这个问题。

顾名思义，Reset CSS 将重置所有内置样式。

最流行的 CSS 重置库是 [Meyer's reset](https://meyerweb.com/eric/tools/css/reset/reset.css)。你可以在这里看到完整的代码。例如，它会将 `body` 的 `margin` 重置为 0：

```css
body {
  margin: 0;
}
```

如果你使用 Chrome 浏览器的开发者工具，检查任何网页的 `body` 元素，你会发现默认情况下它有 8px 的边距，这是我们通常根本不想要的：

```css
body {
  margin: 8px;
}
```

Normalize CSS 是 Reset CSS 的另一种选择。

最受欢迎的库是 [normalize.css](https://necolas.github.io/normalize.css/)。它试图使内置的浏览器样式在不同浏览器之间保持一致。

除此之外，normalize.css 还使某些元素看起来像我们期望的。例如，Chrome、Firefox 和 Safari 浏览器对 `sub` 和 `sup` 标签使用以下样式：

```css
sub {
  vertical-align: sub;
  font-size: smaller;
}

sup {
  vertical-align: super;
  font-size: smaller;
}
```

它修复跨浏览器的显示错误。你可以查看它的[源代码](https://github.com/necolas/normalize.css/blob/master/normalize.css)看到有很多针对不同浏览器的错误修复，比如 IE、Edge、Firefox 等。

如果你使用流行的 CSS 库，你可能不需要在项目中包含 normalize.css。它已经包含在：

- [Bootstrap's reboot](https://github.com/twbs/bootstrap/blob/master/scss/_reboot.scss#L3)
- [Tachyons](https://github.com/tachyons-css/tachyons/blob/master/src/_normalize.css)
- [TailwindCSS](https://unpkg.com/tailwindcss@1.1.4/dist/base.css)
- etc

## 更多资料

- [sanitize.css](https://github.com/csstools/sanitize.css) 是一个 CSS 库，它提供一致的、跨浏览器的 HTML 元素默认样式以及有用的默认样式
- [My Custom CSS Reset](https://www.joshwcomeau.com/css/custom-css-reset/) — Josh Comeau 的 CSS 重置
- [A Modern CSS Reset](https://piccalil.li/blog/a-modern-css-reset/) — Andy Bell 的 CSS 重置
- [cssremedy](https://github.com/jensimmons/cssremedy/) — Jen Simmons 的 CSS Remedy
- [Open Props](https://open-props.style/) — Adam Argyle 的 Open Props，包含一个自定义 normalize.css
- [minireset](https://github.com/jgthms/minireset.css) 是另一个小型的现代 CSS 重置
- [Browser Default Styles](https://browserdefaultstyles.com/) 允许我们获取特定元素的各种渲染引擎的默认样式
- [CSS Reset 与 Sprites](https://github.com/lio-zero/blog/blob/master/CSS/CSS%20Reset%20%E4%B8%8E%20Sprites.md)
