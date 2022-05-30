# 转义 CSS 类名

CSS 类名不能包含 `:` 字符。例如，不可能在 CSS 中声明以下类：

```css
.lg:flex {
  ...;
}
```

但是，我们可以使用 `\` 字符来更正它：

```
.lg\:flex {
  ...
}
```

类名可以像往常一样在 HTML 中使用：

```html
<div class="lg:flex">...</div>
```

在一些 CSS 框架（如 [Tailwind](https://tailwindcss.com/)）中，经常使用 `\` 来转义 CSS 类名。
