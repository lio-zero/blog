# Web 应用中的可访问性

> 维基百科：可访问性是最常用于描述设施或设施，帮助残疾人，如“轮椅”。这可以扩展到盲文标识、轮椅坡道，音频信号在人行横道，轮廓人行道，网站设计，等等。

我们在设计 HTML 时要考虑到可访问性，这一点很重要。

具有可访问的 HTML 意味着残疾人可以使用 Web。它们中有完全失明或视力受损的用户，有听力损失问题的人和许多其他不同的残疾。

如果一个人看不到您的页面，但仍想消费其内容，该怎么办？首先，他们是如何做到的？他们不能使用鼠标，他们使用一种叫做屏幕阅读器的东西。你不必想象。你现在可以试试：谷歌提供免费的 [ChromeVox Chrome 扩展](https://chrome.google.com/webstore/detail/chromevox/kgejglhpjiefppelpmljglcjbhoiplfn/)。可访问性还必须考虑允许工具轻松选择元素或在页面中导航。

网页和 Web 应用程序并不总是以可访问性作为其首要目标之一，也许版本 1 可能发布时无法访问，但可以在发布后使网页可访问。越早越好，但永远不会太晚。

这很重要，对于美国网站来说，有时这是[法定要求](http://www.section508.gov/)，政府或其他公共组织建立的网站必须可以访问的。

以下是一部分您需要考虑的主要事项。

> **注意**：还有其他一些事情需要注意，可能会出现在 CSS 主题中，例如颜色、对比度和字体。或者如何使 SVG 图像可访问。我不在这里谈论他们。

## 使用 HTML 语义

语义 HTML 非常重要，它是您需要注意的主要事项之一。让我举例说明几个常见的场景。

使用正确的标题标签结构很重要。最重要的是 h1，对于不太重要的标题使用较大的数字，但所有相同级别的标题应该具有相同的含义（将其视为树结构）

```txt
h1
h2
h3
h2
h2
h3
h4
```

使用 `strong` 和 `em` 代替 `b` 和 `i`。从视觉上看，它们看起来是一样的，但前 2 个与它们相关的含义更多。`b` 和 `i` 是更多的视觉元素。

> 推荐：[避免使用 b、i、s 和 u 标签](https://github.com/lio-zero/blog/blob/main/HTML/%E9%81%BF%E5%85%8D%E4%BD%BF%E7%94%A8%20b%E3%80%81i%E3%80%81s%20%E5%92%8C%20u%20%E6%A0%87%E7%AD%BE.md)

列表很重要。屏幕阅读器可以检测列表并提供概述，然后让用户选择是否进入列表。

一个表应该有一个 `caption` 描述其内容的标题：

```html
<table>
  <caption>
    Example Caption
  </caption>
  <tr>
    <th>Login</th>
    <th>Email</th>
  </tr>
  <tr>
    <td>user1</td>
    <td>user1@sample.com</td>
  </tr>
  <tr>
    <td>user2</td>
    <td>user2@sample.com</td>
  </tr>
</table>
```

## 对图像使用 alt 属性

所有图像必须具有描述图像内容的 `alt` 标签。这不仅仅是一个好的做法，它是 HTML 标准所要求的，没有它的 HTML 将不会被验证。

```html
<img src="avatar.png" alt="一张图片" />
```

这对搜索引擎也有好处，如果这是您添加它的动机的话。

## 使用 role 属性

`role` 属性允许您为页面中的各种元素分配特定角色。

`role` 没有为大多数元素的默认语义添加任何内容，您可以手动分配许多不同的角色，例如：checkbox、img、list、grid、switch、feed 等等。

为了了解它们，您可以在参考 [MDN](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Roles)。但您不需要为页面中的每个元素分配角色。在大多数情况下，屏幕阅读器可以从 HTML 标签推断。例如，您不需要将角色标签添加到诸如 `nav`、`button` 和 `form` 之类的语义标签中。

让我们以 `nav` 标签为例。您可以使用它来定义页面导航，如下所示：

```html
<nav>
  <ul>
    <li><a href="/">Home</a></li>
    <li><a href="/blog">Blog</a></li>
  </ul>
</nav>
```

如果您被迫使用 `div` 标签而不是 `nav`，您将使用 `navigation` 角色：

```html
<div role="navigation">
  <ul>
    <li><a href="/">Home</a></li>
    <li><a href="/blog">Blog</a></li>
  </ul>
</div>
```

当标签没有传达意义时，`role` 用来分配一个有意义的值。

一些冗余 ARIA 的示例：

```html
<button role="button">关注</button>
<a href="https://github.com/lio-zero" role="link">lio-zero</a>
<input type="checkbox" role="checkbox" />
<input type="radio" role="radio" />
```

HTML5 使用[默认的隐式语义](https://www.w3.org/TR/wai-aria-1.1/#implicit_semantics)定义了一组新的结构和分段元素，这些语义与 ARIA `role` 匹配（在某些情况下）：

- 使用 `role = button` 时，考虑使用 `<button>` 元素，或者其他各种原生 HTML 按钮类型。
- 使用 `role=link` 时，考虑改用 `<a href>` 元素。
- 使用 `role=heading` 和 `aria-level="1-6"`，考虑改用 `<h1>` 到 `<h6>` 元素。
- 使用 `role=list` 和 `role=listitem` 时，考虑改用 `<ol>` 或 `<ul>` 和 `<li>` 元素。
- 使用 `role=listbox` 和 `role=option`，考虑改用 `<select>` 和 `<option>` 元素。
- 使用 `role=checkbox` 或 `role=radio` 时，考虑改用 `<input type="checkbox">` 或 `<input type="radio">` 元素。
- 使用 `role=textbox`，可以考虑使用 `<input type="text">` 或搜索、电子邮件、url 或电话。
- `article`、`aside`、`footer`、`header`、`main`、`nav`、`section` 等等

这意味着在实现后，浏览器将公开该元素的默认隐式语义，因此您不必这样做。

## 使用 tabindex 属性

`tabindex` 属性允许您更改按 **Tab** 键选择**可选**元素的顺序。默认情况下，只有链接和表单元素可以通过使用 **Tab** 键进行导航来选择它们（您不需要在它们上设置 `tabindex`）。

添加 `tabindex="0"` 使元素可选择：

```html
<div tabindex="0">...</div>
```

使用 `tabindex="-1"` 可以从基于选项卡的导航中删除一个元素，这非常有用。

## 使用 aria 属性

ARIA 是一个首字母缩略词，表示可访问的富互联网应用程序，并定义了可应用于元素的语义。

> **[Accessible Rich Internet Applications（ARIA）**](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Roles/Region_role) 是能够让残障人士更加便利的访问 Web 内容和使用 Web 应用（特别是那些由 JavaScript 开发的）的一套机制。

ARIA 通过 HTML 属性为屏幕阅读器提供了额外的信息。其不影响元素如何被渲染在浏览器中。

### aria-label

该属性用于添加一个字符串来描述一个元素。

示例：

```html
<p aria-label="产品描述">...</p>
```

### aria-labelledby

此属性设置当前元素与标记它的元素之间的相关性。

如果您知道如何将 `input` 元素与 `label` 元素相关联，这是类似的。

我们传递描述当前元素的项 `id`。

示例：

```html
<h3 id="description">The description of the product</h3>

<p aria-labelledby="description">...</p>
```

### aria-describedby

这个属性让我们将一个元素与另一个用作描述的元素相关联。

示例：

```html
<button aria-describedby="payNowDescription">立即付款</button>

<div id="payNowDescription">点击按钮将发送到我们的表单！</div>
```

### 使用 aria-hidden 隐藏内容

有一些内容对于阅读障碍的人来说是无用的，例如：视觉效果的暗模式、图标、图片等。

添加 `aria-hidden="true"` 属性将告诉屏幕阅读器忽略该元素。

## 更多资料

- [A Complete Guide To Accessible Front-End Components](https://www.smashingmagazine.com/2021/03/complete-guide-accessible-front-end-components/)
- [Using ARIA](https://w3c.github.io/using-aria/)
- [MDN: ARIA](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA)
- [Accessibility](https://web.dev/accessibility/)
- [WebAIM](https://webaim.org/)
- [On HTML belts and ARIA braces (The Default Implicit ARIA semantics they didn’t want you to know about)](http://html5doctor.com/on-html-belts-and-aria-braces/)
- [using-aria](https://w3c.github.io/using-aria/)
- [Tools for Developing Accessible Websites (bitsofco.de)](https://bitsofco.de/tools-for-developing-accessible-websites/)
- [Why you should use focus styles - LogRocket Blog](https://blog.logrocket.com/why-you-should-use-focus-styles-193d58672c5c/)
- [Four Ways Design Systems Can Promote Accessibility — and What They Can’t Do](https://24ways.org/2019/four-ways-design-systems-can-promote-accessibility/)
- [The 6 Most Common Accessibility Problems (and How to Fix Them)](https://blog.scottlogic.com/2020/07/02/6-most-common-accessibility-problems.html#empty-links-and-empty-buttons)
- [A practical guide to accessibility for forms](https://blog.logrocket.com/a-practical-guide-to-accessibility-for-forms/)
- [WAI-ARIA](http://www.w3.org/WAI/intro/aria)
- [在 HTML 中使用 ARIA 的规则](https://github.com/lio-zero/blog/blob/main/HTML/%E5%9C%A8%20HTML%20%E4%B8%AD%E4%BD%BF%E7%94%A8%20ARIA%20%E7%9A%84%E8%A7%84%E5%88%99.md)
- [webhint](https://webhint.io) 是一个可自定义的整理工具，可通过检查代码中的最佳做法和常见错误来帮助您提高网站的可访问性，速度，跨浏览器兼容性以及其他功能。
- [Accessibility Tutorial](https://www.w3schools.com/accessibility/index.php)
