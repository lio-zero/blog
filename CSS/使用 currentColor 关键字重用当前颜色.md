# 使用 currentColor 关键字重用当前颜色

我们可以一次性为 `color` 属性定义一个值，并将其与 `currentColor` 关键字一起重用，而不是在几个地方重复使用颜色。

```css
/* ❌ */
div {
  color: #d1d5db;
  background-image: linear-gradient(to bottom, #d1d5db, #fff);
}

/* ✅ */
div {
  color: #d1d5db;
  background-image: linear-gradient(to bottom, currentColor, #fff);
}
```

因为元素的 `color` 属性（如果未指定）是从其父元素继承的，所以我们可以在元素的子元素中使用 `currentColor` 关键字。

例如，假设我们希望链接的颜色与其容器（给定的 `div` 元素）相同：

```css
/* 不好的做法：在三个地方声明相同的颜色 */
div {
  color: #fff;
}

div a {
  border-bottom: 1px solid #fff;
  color: #fff;
  text-decoration: none;
}

/* 好的做法 */
div {
  color: #fff;
}

div a {
  border-bottom: 1px solid currentColor;
  color: currentColor;
  text-decoration: none;
}
```

我们经常在 camelCase 格式中使用 `currentColor` 关键字。但是，CSS 不区分大小写，这意味着`currentColor`、`CurrentColor` 甚至 `Currentcolor` 都是有效的关键字，并且与 `currentColor` 具有相同的效果
