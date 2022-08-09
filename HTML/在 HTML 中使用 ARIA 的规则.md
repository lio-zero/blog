# 在 HTML 中使用 ARIA 的规则

## 什么是 ARIA？

> [**Accessible Rich Internet Applications（WAI-ARIA，简称 ARIA）**](https://www.w3.org/WAI/intro/aria)是能够让残障人士更加便利的访问 Web 内容和使用 Web 应用（特别是那些由 JavaScript 开发的）的一套机制。最值得注意的是，它包含了一组属性，我们可以添加到 HTML 元素中，将更多的语义信息嵌入其中，这些信息可以被辅助技术读取。

- ARIA 可以通过 HTML 属性为屏幕阅读器提供了额外的信息。其不影响元素如何被渲染在浏览器中。
- ARIA `role` 没有为大多数元素的默认语义添加任何内容。
- 您可以通过遵循 ARIA 标准（例如：HTML 语义，使用 `alt` 属性并以预期的方式使用 `[role = button]`）来使您的网站更易于访问

一个例子：

```html
<style>
  [role='note'] {
    padding: 10px;
    border: 1px solid;
    border-left: 5px solid rebeccapurple;
    color: rebeccapurple;
    border-radius: 5px;
  }
</style>
<section>
  <div role="note">
    Lorem ipsum dolor sit amet consectetur adipisicing elit. Quo illum cum
    totam.
  </div>
</section>
```

它将向用户代理暗示，这是一个 `note`，用来记录一些您需要留意的事情。

尽管 ARIA 非常有用，但我们必须注意何时以及如何使用它。

## 使用 HTML5 语义以支持 ARIA

> 您应该使用已经内置了所需语义和行为的原生 HTML 元素或属性，而不是重新使用 HTML 元素并添加 ARIA `role`、状态或属性使其可访问。

在 HTML 中使用 ARIA 的首要规则是尽量不要在 HTML 中使用 ARIA（如果不需要的话）。HTML5 语义元素为我们提供了一系列具有隐式含义的元素，类似于我们可以使用 ARIA 定义的显式含义，常被称为元素的[默认隐式 ARIA 语义](https://www.w3.org/TR/wai-aria-1.1/%23implicit_semantics)。

因此，只要有可能，我们就应该使用 HTML 语义元素来代替 ARIA 属性。

例如下面这个例子：

```html
<div role="button">Click Me</div>
```

上面的写法是错误的，你不应该使用 `<div>` 和 ARIA `role` 来创建一个按钮。

我们应该只使用一个明确其语义的 `<button>` 元素，就像这样：

```html
<button>Click Me</button>
```

## 不要使用 role 属性改变语义元素的含义

> 不要改变原生语义，除非你真的必须这样做。

正如我们提到的，许多 HTML 语义元素对它们都有隐含的意义。

例如：

- 当我们使用 `<button>` 元素时，它向用户代理暗示这是一个交互元素（通过光标单击、回车键或空格键进行交互），它将触发页面上的一些交互。

- 如果我们使用一个 `<a>` 元素，那么对用户代理来说，与该元素交互（通过光标单击或回车键）会将用户从页面导航到另一个位置，或者导航到同一页面上的不同位置。

由于这些元素中的许多元素都具有隐含的含义，因此建议不要使用 ARIA `role` 来更改它们。以下是一个例子：

```html
<h1 role="button">Heading Button</h1>
```

以上写法是错误的，我们应该使用适当的元素，而不是重新调整语义元素的用途。

```html
<h1>
  <button>Heading Button</button>
</h1>
```

或者，我们可以将 ARIA `role` 应用到一个没有任何含义的元素上（你觉得有必要的话），如 `<span>`。

```html
<h1><span role="button">Heading Button</span></h1>
```

其他一些冗余 ARIA 的示例：

```html
<button role="button">戳我</button>
<a href="https://www.baidu.com" role="link">你敢点我吗？</a>
<input type="checkbox" role="checkbox" />
<input type="radio" role="radio" />
```

HTML5 使用默认的隐式语义定义了一组新的结构和分段元素，这些语义与 [ARIA `role`](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Roles/Region_role) 匹配（在某些情况下）：

- 使用 `role=button` 时，考虑使用 `<button>` 元素，或者其他各种原生 HTML 按钮类型。
- 使用 `role=link` 时，考虑改用 `<a href>` 元素。
- 使用 `role=heading` 和 `aria-level="1-6"`，考虑改用 `<h1>` 到 `<h6>` 元素。
- 使用 `role=list` 和 `role=listitem` 时，考虑改用 `<ol>` 或 `<ul>` 和 `<li>` 元素。
- 使用 `role=listbox` 和 `role=option`，考虑改用 `<select>` 和 `<option>` 元素。
- 使用 `role=checkbox` 或 `role=radio` 时，考虑改用 `<input type="checkbox">` 或 `<input type="radio">` 元素。
- 使用 `role=textbox`，可以考虑使用 `<input type="text">` 或搜索、电子邮件、URL 或电话。
- 其他一些语义元素：`article`、`aside`、`footer`、`header`、`main`、`nav`、`section` 等等。

这意味着在实现后，浏览器将公开该元素的默认隐式语义，因此您不必这样做。

## 交互式 ARIA 元素必须被所有媒体访问

> 所有交互式 ARIA 控件必须与键盘一起使用。

在元素上使用 ARIA `role` 并不足以真正改变元素的 `role`。例如，如果我们试图将一个 `<div>` 更改为一个 `<button>`，则需要手动添加适合 `<button>` 的交互功能。

在 ARIA 指南中，有一个每个元素应该具有的功能列表。例如，一个有效的按钮必须满足以下要求：必须可以用光标、回车键、空格键单击（配合 JS 可以轻松实现）。

在使用 ARIA `role` 时，我们需要了解这些需求。使元素看起来像按钮并不能使它成为按钮。我们需要考虑所有媒介中的用户如何与元素交互。

## 对可见的可聚焦元素使用适当的 Role

> 不要在可见的可聚焦元素上使用 `role="presentation"` 或 `aria-hidden="true"`。

ARIA `role="presentation"` 属性意味着元素仅用于视觉目的，并且不以任何方式交互。`aria-hidden="true"` 属性表示元素不应可见。

当我们使用这些属性时，我们需要知道它们所应用到的元素以及它们的可见性和交互状态。例如，这两个按钮都会导致一些用户关注他们不存在的元素。

```html
<button role="presentation">Click Me</button>
<button aria-hidden="true">Click Me</button>
```

以上写法都是不正确，这些属性只能应用于非交互或不可见的元素。

```html
<button role="presentation" tabindex="-1">Don't Click Me</button>
<button aria-hidden="true" style="display: none;">Don't Click Me</button>
```

## 交互元素必须具有可访问的名称

> 所有交互元素必须具有可访问的名称。

可以与之交互的元素，例如按钮和链接，需要有一个**可访问的名称（Accessible Name）**。**可访问名称**是在可访问性 API 属性具有有效值时确定的。

可以根据元素的类型指定元素的可访问名称。例如，表单输入通常从相应的 `<label>` 元素获取其可访问的名称。

```html
<label>
  Username
  <input type="text" />
</label>

<label for="password">Password</label>
<input type="password" id="password" />
```

其他元素，例如按钮和链接，可以从它们的文本内容或标签属性中获得它们的可访问名称。

## 更多资源

- [MDN：ARIA](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA) —— MDN 上丰富的 ARIA 资料
- [在 HTML 和 ARIA 大括号上（默认的隐式 ARIA 语义，他们不想让你知道）](http://html5doctor.com/on-html-belts-and-aria-braces/)
- [HTML5 – W3C 建议书 2014 年 10 月 28 日](https://www.w3.org/TR/html5/)
- [在 HTML 中使用 ARIA 的注意事项](http://w3c.github.io/aria-in-html/)
