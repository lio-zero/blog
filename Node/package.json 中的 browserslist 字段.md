# package.json 中的 browserslist 字段

**Autoprefixer** 的作者 Andrey Sitnik 建议通过 Web 应用程序 `package.json` 文件的`browserslist` 字段指定支持的浏览器列表。除 **Autoprefixer** 外，以下工具也使用该字段:

- [**babel-preset-env**](https://github.com/babel/babel-preset-env)（在下一个主要版本中）— 仅添加目标浏览器所需的 ECMAScript 2015+ polyfill
- [**eslint-plugin-compat**](https://github.com/amilajack/eslint-plugin-compat) — 检查 Web 应用的 JavaScript 代码是否被所有目标浏览器支持
- [**stylelint-no-unsupported-browser-features**](https://github.com/ismay/stylelint-no-unsupported-browser-features) — 检查 Web 应用的 CSS 代码是否被所有目标浏览器支持
- [**postcss-normalize**](https://github.com/csstools/postcss-normalize) — 仅包括目标浏览器所需的 normalize.css 部分

一个示例配置：

```json
{
  "browserslist": ["last 2 versions", ">1%"]
}
```
