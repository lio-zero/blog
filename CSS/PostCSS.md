# PostCSS

> [PostCSS](https://postcss.org/)（后预处理器）是一种使用 JS 插件转换样式的工具。这些插件可以 lint 你的 CSS，支持变量和 `mixins`，转换未来的 CSS 语法，内联图像等。

## 与其他 CSS 预处理器有何不同？

PostCSS 相比 Sass 或 Less 等 CSS 预处理器提供的主要好处是能够选择自己的路径，并选择所需的功能，同时添加新功能。Sass 或 Less 是固定的，您可以获得许多您可能使用或不使用的功能，但您无法扩展它们。

您完全可以使用 PostCSS 代替 Sass 或 Less，但如果你不喜欢，您仍然可以使用 Sass 或 Less，并使用 PostCSS 执行 Sass 无法执行的其他操作，如自动刷新或 linting。

你可以编写自己的 [PostCSS 插件](https://github.com/postcss/postcss/blob/main/docs/writing-a-plugin.md)来做任何你想做的事情。

## 安装 PostCSS

全局安装 [postcss-cli](https://github.com/postcss/postcss-cli)：

```bash
pnpm i -g postcss-cli
```

输入以下命令确保其正常工作：

```bash
postcss --help
```

## 常用的插件

### Autoprefixer

[Autoprefixer](https://github.com/postcss/autoprefixer) 解析 CSS 并确定某些规则是否需要供应商前缀。

它根据 [Can I Use](http://caniuse.com/) 数据执行此操作，因此您不必担心某个功能是否需要前缀，或者您使用的前缀是否因为过时而不再需要。

你可以写出更简洁的 CSS。

例子：

```css
a {
  display: flex;
}
```

被编译为：

```css
a {
  display: -webkit-box;
  display: -webkit-flex;
  display: -ms-flexbox;
  display: flex;
}
```

### PostCSS Preset Env

[PostCSS Preset Env](https://github.com/csstools/postcss-preset-env) 是 CSS 中的 Babel，允许您将现代 CSS 转换为大多数浏览器都能理解的内容，根据您的目标浏览器或运行时环境确定您需要的 polyfill：

- 它使用 Autoprefixer 添加前缀（因此，如果使用它，则无需直接使用 Autoprefixer）
- 它允许您使用 CSS 变量
- 它允许您使用嵌套，就像在 Sass 中一样
- etc

### CSS Modules

[PostCSS Modules](https://github.com/css-modules/postcss-modules) 允许您使用 CSS 模块。

CSS 模块不是 CSS 标准的一部分，但它们是具有作用域选择器的构建步骤。

### csslint

Linting 可以帮助您编写正确的 CSS，避免错误或陷阱。[Stylelint](http://stylelint.io/) 插件允许您在构建时删除 CSS。

### cssnano

[cssnano](https://cssnano.co/) 缩小了 CSS 并对代码进行了优化，以在生产中交付最少量的代码。

### 其他有用的插件

[PostCSS Plugins](https://github.com/postcss/postcss/blob/main/docs/plugins.md#postcss-plugins) 提供了可用插件的完整列表。

其中一些：

- [LostGrid](https://github.com/peterramsing/lost) 是一个 PostCSS 网格系统
- [postcss-nested](https://github.com/postcss/postcss-nested) 提供了使用 Sass 嵌套规则的能力
- [postcss-nested-ancestors](https://github.com/toomuchdesign/postcss-nested-ancestors) 引用嵌套 CSS 中的任何祖先选择器
- [postcss-simple-vars](https://github.com/postcss/postcss-simple-vars) 使用类似 Sass 的变量
- [PreCSS](https://github.com/jonathantneal/precss) 为您提供了 Sass 的许多功能，这是最接近于完整的 Sass 替代品的功能

## 进一步阅读

- [An Introduction to PostCSS](https://www.sitepoint.com/an-introduction-to-postcss/)
- [How to Use PostCSS as a Configurable Alternative to Sass](https://www.sitepoint.com/postcss-sass-configurable-alternative/)
- [PostCSS — A Comprehensive Introduction](https://www.smashingmagazine.com/2015/12/introduction-to-postcss/)
- [From Sass to PostCSS](https://tylergaw.com/blog/sass-to-postcss/)
