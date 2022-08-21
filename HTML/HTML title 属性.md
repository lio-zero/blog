# HTML title 属性

> **先总结**：除非在特殊情况下，否则不要使用 [`title`](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/title) 属性。

HTML `title` 全局属性包含表示与其所属元素相关的建议信息的文本。也就是指定元素的提示文本。

当鼠标移动到带有 `title` 属性的元素上时，提示文本将作为工具提示（tooltip）显示出来。可以说，`title` 是对该元素的描述和进一步的说明。

但在一些情况下，它可能是不必要的。

我们都知道，`title` 属性通常被用于可访问性和 SEO 的优化。但对于屏幕阅读器用户来说，`title` 属性中包含的内容通常是不必要的、多余的，甚至可能没有使用。在大多数情况下，放置在 `title` 属性中的内容对（可能的）大多数用户是隐藏的。如果信息对大多数用户都是隐藏的，那么可能就没有存在的必要了。

以下是常见适合使用 `title` 属性的情况：

- 对于 `<frame>` 和 `<iframe>` 元素
- 用于在文本标签多余时提供标签

如果想要在 `<img>` 标签上使用 `title`，请保持与 `alt` 相同的值。

根据文本替代计算的预期行为，计算文本替代的优先级应该是：

- [`aria-labelledby`](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/ARIA_Techniques/Using_the_aria-labelledby_attribute) —— 该属性用来表明某些元素的 `id` 是某一对象的标签。
- [`aria-label`](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/ARIA_Techniques/Using_the_aria-label_attribute) —— 该属性用来给当前元素加上的标签描述，接受字符串作为参数。
- `alt`
- `title`

在使用上述两种或两种以上的情况下，列表中最高的就成为所使用的。考虑以下示例：

```html
<img src="logo.png" alt="" title="logo" />
```

在这种情况下，`alt` 实际上成为了首要方案，因为它的优先级更高。因此，即使 `title` 拥有有用的内容，它也不会被使用，因为 `alt` 在那里。

对于一个普遍可靠的文本替代图像，`alt` 属性应该是首选方法。在提供 `title` 属性的情况下，它应该与 `alt` 具有相同的值。

## 更多资料

- [The Trials and Tribulations of the Title Attribute](https://www.24a11y.com/2017/the-trials-and-tribulations-of-the-title-attribute/)
- [Using the HTML title attribute](https://developer.paciellogroup.com/blog/2013/01/using-the-html-title-attribute-updated/)
