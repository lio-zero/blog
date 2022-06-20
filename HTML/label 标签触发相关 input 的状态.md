# label 标签触发相关 input 的状态

`input` 元素的伪类 `:hover` 和 `:active` 也会在相关被悬停时被触发。

```html
<label for="foo">Foo</label>
<p>Some more content here</p>
<input id="foo" />
```

```css
#foo:hover {
  background: yellow;
}
```

在上面的例子中，当您将鼠标悬停在 `label` 标签上时，输入框将显示为黄色。

> **注意**：我们在前面的文章说过，当您单击一个相关标签时，输入框上的单击处理程序也会被触发。
