# 对于特定于语言的样式，在 lang 属性选择器上使用 lang 伪类

HTML 文档的内容可以是多种不同的语言。

为了指定文档的主要语言，我们可以在根元素上使用 `lang` 属性。

```html
<html lang="zh"></html>
```

我们还可以在页面中使用 `lang` 属性来划分与文档主语言不同语言的特定元素或部分。

```html
<html lang="zh">
  <head></head>
  <body>
    <p lang="en">I am Superman</p>
  </body>
</html>
```

我们为页面上的 `p` 元素设置了 `lang` 属性，并设置了 `en`（英语）语言，如果我们想要给它设置样式，我们可以使用 `[lang]` 属性选择器来选择具有特定语言属性的所有元素或元素的子元素。

```css
[lang='en'] p {
  font-size: 1.2em;
  color: plum;
}
```

## `[lang]` 属性选择器不知道该元素的语言

使用基于 `[lang]` 属性选择器的特定于语言的样式的问题在于，选择器实际上并不知道元素的语言。它就像任何其他属性选择器一样。

如果一个文档包含多个不同语言的嵌套元素，这可能会成为一个问题。

让我们看下面这一节。该节具有法语 `lang` 属性，但在该节中，我们有一个西班牙语引用。

```html
<section lang="zh">
  <p>注意：xxx</p>
  <blockquote lang="en">
    <p>I am Superman</p>
  </blockquote>
</section>
```

在我们的 CSS 中，我们可能只是简单地用一个特定的 `lang` 属性及其后代来设置所有元素的样式。

```css
[lang='en'] p {
  padding: 10px;
  border: 1px solid;
  border-left: 5px solid rebeccapurple;
  color: rebeccapurple;
  border-radius: 5px;
}

[lang='zh'] p {
  padding: 10px;
  border: 1px solid;
  border-left: 5px solid plum;
  color: plum;
  border-radius: 5px;
}
```

效果如下：

![设置样式](https://upload-images.jianshu.io/upload_images/18281896-8cd0f01ff1286191.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

CSS 没有优先考虑更接近的父级的概念。因此，无论哪个 `lang` 属性谁更接近谁，都会根据我们声明的 CSS 样式的顺序设置样式。所以这里的样式设置为了 `zh`，但我们本想区分两者。该怎么办？

接下来就需要使用到 `:lang` 伪类选择器了。

## `:lang` 伪类

`:lang` 是一个语言选择器，会根据 `html` 设置的语言应用对应的样式。可以有两种方式设置 `lang` 属性的样式：

- 它使用所选元素的实际语言
- 它可以应用于任何元素，而不是直接基于 `lang` 属性

### `:lang` 和内容语言

`:lang` 伪类用于根据元素的内容语言选择元素。元素的内容语言由以下三个因素决定：

- 任何 `lang` 属性
- 元标签，例如 `<meta http-equiv="content-language" content="en">`
- HTTP 头，例如 `Content-language: en`

这意味着 `:lang` 伪类即使在没有指定 `lang` 属性的情况下也可以使用。

### `:lang` 和嵌套

作为一个伪类，`:lang` 最好用于特定的元素，而不是子元素。

使用与上面相同的示例，我们可以切换到使用 `:lang()` 属性来选择 `p` 元素。但是，我们不必选择指定语言的子 `p` 元素，而是可以选择本身属于指定语言的 `p` 元素。

```css
p:lang(zh) {
  padding: 10px;
  border: 1px solid;
  border-left: 5px solid rebeccapurple;
  color: rebeccapurple;
  border-radius: 5px;
}

p:lang(en) {
  padding: 10px;
  border: 1px solid;
  border-left: 5px solid plum;
  color: plum;
  border-radius: 5px;
}
```

即使 `p` 元素本身没有 `lang` 属性，我们仍然可以使用伪类，因为 `p` 元素的内容语言是从其父级继承的。

## `lang` 属性不能应用的元素

`lang` 属性不能应用于以下元素（不包含不推荐使用的标签）：

- `br`
- `iframe`
- `script`
- `base`
- `param`

## HTML 中的 `lang` 属性有什么作用？

- 根据 `lang` 属性来设定不同语言的 `css` 样式，或者字体
- 告诉搜索引擎做精确的识别
- 让语法检查程序做语言识别
- 帮助翻译工具做识别
- 帮助网页阅读程序做识别等等
