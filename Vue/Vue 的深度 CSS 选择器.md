# Vue 的深度 CSS 选择器

我们知道，当 `<style>` 标签有 `scoped` 属性时，它的 CSS 只作用于当前组件中的元素，父组件的样式将不会渗透到子组件。 如果你希望 `scoped` 样式中的一个选择器能够作用得“更深”，例如影响子组件或修改第三方库组件的样式时，你可以使用深度选择器。

在 Vue2.x 有三种选择：

- `>>>` — 有些例如 SASS 预处理器无法正常解析，使用前需注意
- `/deep/` 和 `::v-deep` — 支持预处理器解析，它们都是 `>>>` 的别名，但在 Vue 3.x 将不在被支持。

如果使用后面两种，Vue 会提示你：

> **[@vue/compiler-sfc] ::v-deep usage as a combinator has been deprecated. Use :deep(<inner-selector>) instead.**

在 Vue3.x 中以上语法将替换为 `:deep()` 和 `::v-deep()` 的任意一种。

另外，Vue3 还提供了**插槽选择器**和**全局选择器**

- `:slotted()` — 默认情况下，作用域样式不影响由 `<slot/>` 渲染的内容，因为它们被认为是由传入它们的父组件所拥有。要明确定位插槽内容，请使用 `:slotted` 伪类
- `:global()` — 如果只想全局应用一个规则，可以使用 `global` 伪类，而不是创建另一个 `<style>`
