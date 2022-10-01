# 避免使用 CSS @import

`@import` 函数允许我们包含来自外部文件的样式。当我们的项目有很多样式时，它非常有用。我们不需要创建一个文件来定义所有样式，而是可以将它们拆分为多个文件，并将它们组合到主文件中。

```js
/* 主文件 */
@import 'common.css';
@import 'components.css';
@import 'pages.css';
...
```

使用 `@import` 可以使我们的样式更有条理，更易于维护。但是，在继续渲染页面之前，浏览器必须逐个下载并解析每个 CSS 文件。CSS 文件按顺序下载，而不是并行下载。

它还会根据 CSS 文件的数量来降低网站的速度。

在风格仍然井然有序的情况下，有几种方法可以解决这些问题。

## 使用 CSS 预处理器

我们可以使用 CSS 预处理器，如 [Less](http://lesscss.org/)、[SASS](https://sass-lang.com/)。它们不仅提供了将 `@import` 作为普通 CSS 使用的功能，而且还可以将样式合并到单个最终 CSS 文件中。

## 使用多个 link 标签

每个 CSS 都可以通过单独的 `link` 标签下载，如下所示：

```html
<head>
  <link rel="stylesheet" type="text/css" href="common.css" />
  <link rel="stylesheet" type="text/css" href="components.css" />
  ...
</head>
```

> **注意**：在旧版本的 Internet Explorer 中，`@import` 函数的行为与在页面底部插入目标 CSS 的行为相同

这里还涉及一道常见的面试题。

## 页面导入样式时，使用 `link` 和 `@import` 有什么区别？

- `link` 属于 XHTML 标签，除了加载 CSS 外，还能用于定义 RSS，定义 `rel` 连接属性等作用；而 `@import` 是 CSS 提供的，只能用于加载 CSS。
- 页面被加载的时，`link` 会同时被加载，而 `@import` 引用的 CSS 会等到页面被加载完再加载。
- `@import` 是 CSS2.1 提出的，只在 IE5 以上才能被识别，而 `link` 是 XHTML 标签，无兼容问题。
- `link` 引入样式的方式权重高于 `@import` 的权重。
