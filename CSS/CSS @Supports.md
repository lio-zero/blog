# CSS @Supports

CSS [`@supports`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/@supports) 指令会像 `@media` 查询一样出现在 CSS 代码中：

```css
@supports (prop: value) {
  /* more styles */
}
```

CSS `@supports` 允许开发人员以多种不同的方式检查样式支持。

例如：检查是否支持 `display: flex`：

```css
@supports (display: flex) {
  div {
    display: flex;
  }
}
```

## not 关键字

`@supports` 可以与 `not` 关键字一起使用，以检查是否不支持：

```css
@supports not (display: flex) {
  div {
    float: left;
  }
}
```

## 多重检查和条件

可以通过 `or` 和 `and` 链接进行多个 CSS 属性检查：

```css
/* or */
@supports (display: -webkit-flex) or (display: -moz-flex) or (display: flex) {
  /* use styles here */
}

/* and */
@supports (display: flex) and (-webkit-appearance: caret) {
  /* something crazy here */
}
```

两者一起使用：

```css
/* and 和 or */
@supports ((display: -webkit-flex) or (display: -moz-flex) or (display: flex))
  and (-webkit-appearance: caret) {
  /* use styles here */
}
```

`@supports` 结构的条件模式与 `@media` 的条件模式匹配。

## JavaScript `CSS.supports`

CSS `@supports` 对应的 JavaScript 是 [`CSS.supports`](https://developer.mozilla.org/zh-CN/docs/Web/API/CSS/supports)。`CSS.supports` 提供了两种使用方法。

- 第一种使用方法包括提供两个参数：一个用于属性，另一个用于值：

```js
const supportsFlex = CSS.supports('display', 'flex')
```

- 第二种用法包括简单地提供要分析的整个字符串：

```js
const supportsFlexAndAppearance = CSS.supports(
  '(display: flex) and (-webkit-appearance: caret)'
)
```

您可以通过这两种方法来检查 CSS 支持情况，它避免了在临时节点上进行属性检查，也避免了为检查支持而构建字符串。

在使用 `supports` 的 JavaScript 方法之前，首先检测功能是很重要的。Opera 使用了一个不同的方法名，因此会抛出一些东西：

```js
const supportsCSS = !!(
  (window.CSS && window.CSS.supports) ||
  window.supportsCSS ||
  false
)
```

## @Supports 使用

在大多数情况下，`@supports` 的最佳用途是将一组较旧的样式设置为备份，然后取消这些样式，并在支持给定属性的情况下进行增强。

`@supports` 的一个优秀用例是布局。一些边缘浏览器现在提供对 `flexbox` 的支持，而其他浏览器则落后。在这种情况下，您可以编写以下代码：

```css
section {
  float: left;
}

@supports (display: -webkit-flex) or (display: -moz-flex) or (display: flex) {
  section {
    display: -webkit-flex;
    display: -moz-flex;
    display: flex;
    float: none;
  }
}
```

随着开发人员有更多时间尝试新的 `@supports` 功能，其他良好的用例也会出现。

> [MDN 提供了更多示例](https://developer.mozilla.org/en-US/docs/Web/CSS/@supports#examples)
