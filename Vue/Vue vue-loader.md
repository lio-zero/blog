# vue-loader

[vue-loader](https://github.com/vuejs/vue-loader) 作为 webpack 中一个为解析 `.vue` 文件的 loader。主要的作用是将 Vue 单文件组件（SFC，Single-File Component）解析为 vue runtime 可识别的组件模块。

有了 vue-loader，我们就可以在项目中编写 SFC 格式的 Vue 组件。

## 原理

vue-loader 使用 [`@vue/compiler-sfc`](https://www.npmjs.com/package/@vue/compiler-sfc) 将 SFC 中的内容拆分为 `template`、`script` 和 `style` 三个**虚拟模块**，然后分别匹配 webpack 配置中对应的 `rules`。比如：

- `script` 模块会匹配所有跟处理 JavaScript 或 TypeScript 相关的 loader。
- `template` 中的内容会通过 vue template compiler 转换为 `render` 函数后合并到 `script` **虚拟模块**中。
- `scoped style` 会经过一些处理策略的处理，成为只匹配特定元素的私有样式。

## 更多资料

详细运作模式查阅 [vue-loader README](https://github.com/vuejs/vue-loader#what-is-vue-loader)。

关于 `scoped style` 原理回答范式，可以查看 👉 [你知道 Vue scoped 原理吗？这波你在第几层？](https://juejin.cn/post/7098569051860893709#comment)。
