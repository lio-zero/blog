# 省略 HTML 布尔属性的值

有一些 HTML 布尔属性，如 `checked`、`disabled`、`readonly`、`required`、`selected` 等。

根据 [HTML 规范](https://html.spec.whatwg.org/#boolean-attribute)，布尔属性有三种可能的声明。所有这些都具有相同的效果：

```html
<input readonly />
<input readonly="" />
<input readonly="readonly" />
```

`true` 和 `false` 是无效值：

```html
<!-- 不允许 -->
<button disabled="true">...</button>
<button disabled="false">...</button>
```

表示 `false` 值的唯一方法是删除该属性。因此，为了避免错误和误导性用法，建议不为布尔属性赋值：

```html
<input readonly />
```
