# 只为 Firefox 编写 CSS 规则

如果您想添加一些 CSS 规则来修复 Firefox 上的问题，那么这个技巧可能很有用。

有两种检测 Firefox 的方法：

```css
@-moz-document url-prefix() {
  h1 {
    color: blue;
  }
}

/* 使用 `@support` */
@supports (-moz-appearance: none) {
  h1 {
    color: blue;
  }
}
```

上面的示例代码将为 Firefox 上的 `h1` 添加蓝色。

> **解释**
>
> 任何以 CSS 开头的规则 `@-moz-` 都是 Gecko 引擎特定的规则，而不是标准规则。也就是说，它是 Mozilla 特定的扩展。
>
> `url-prefix` 规则将包含的样式规则应用于 URL 以其开头的任何页面。不带 URL 参数使用时，`@-moz-document url- prefix()` 它适用于**所有**页面。实际上，这是仅用于 Gecko（Mozilla Firefox）的 [CSS hack](http://en.wikipedia.org/wiki/CSS_filter)。所有其他浏览器将忽略样式。
