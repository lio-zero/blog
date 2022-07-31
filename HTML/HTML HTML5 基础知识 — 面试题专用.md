# HTML HTML5 基础知识 — 面试题专用

## DTD 介绍

- DTD（Document Type Definition 文档类型定义）是一组机器可读的规则，它们定义 XML 或 HTML 的特定版本中所有允许元素及它们的属性和层次关系的定义。在解析网页时，浏 览器将使用这些规则检查页面的有效性并且采取相应的措施。
- DTD 是对 HTML 文档的声明，还会影响浏览器的渲染模式（工作模式）

## 什么是 HTML？

HTML 是网页的基础。它代表**超文本标记语言**。

作为一个 Web 用户，当你访问本页面时，这个过程看起来是这样的：

- 在设备上打开 Web 浏览器应用程序。
- 单击指向此页面的链接或直接在地址栏中键入 URL。
- 运行此网站的 Web 服务器接收来自您和您的浏览器的请求，并向浏览器发送一些 HTML 代码。
- 浏览器下载 HTML 代码，对其进行解析，并在屏幕上显示结果。

这就是为什么这个页面会显示你现在正在阅读的内容。HTML 是用来表示构成此页面的各种元素的语言。

## SGML 、 HTML 、XML 和 XHTML 的区别？

- SGML 是标准通用标记语言，是一种定义电子文档结构和描述其内容的国际标准语言， 是所有电子文档标记语言的起源
- HTML（HyperText Markup Language）是超文本标记语言，它定义了网页内容的含义和结构。
- XML 是可扩展标记语言是未来网页语言的发展方向，XML 和 HTML 的最大区别就在于 XML 的标签是可以自己创建的，数量无限多，而 HTML 的标签都是固定的而且数量有限。
- XHTML 是一个基于 XML 的标记语言，他与 HTML 没什么本质的区别，但他比 HTML 更加严格。
- 为了规范 HTML，W3C 结合 XML 制定了 XHTML1.0 标准，这个标准没有增加任何新的标签，只是按照 XML 的要求来规范 HTML。两者最主要的区别是：
  - 文档顶部 `doctype` 声明不同，XHTML 的 doctype 顶部声明中明确规定了 xhtml DTD 的写法
  - 元素必须始终正确嵌套
  - 标签必须始终关闭
  - 标签名必须小写
  - 特殊字符必须转义
  - 文档必须有根元素
  - 属性值必须用双引号 `""` 括起来
  - 禁止属性最小化（例如，必须使用 `checked="checked"` 而不是 `checked`）

## DOCTYPE 有什么用？

- [`<!DOCTYPE>`](https://developer.mozilla.org/en-US/docs/Glossary/Doctype) 声明位于 HTML 文档中的第一行，处于 `<html>` 标签之前。告知浏览器的解析器使用标准模式渲染文档。DOCTYPE 不存在或格式不正确会导致文档以兼容模式呈现。

```js
// 返回当前文档关联的文档类型定义（DTD），如果当前文档没有 DTD，则该属性返回 null。
document.doctype
```

## 标准模式与兼容模式各有什么区别？它们有何意义？

- **标准模式**：又称严格模式，是指浏览器按照 W3C 标准解析代码。
  - 标准模式的渲染方式和 JS 引擎的解析方式都是以该浏览器支持的最高标准运行。
- **兼容模式**：又称怪异模式或混杂模式，是指浏览器用自己的方式解析代码。
  - 在兼容模式中，页面以宽松的向后兼容的方式显示，模拟老式浏览器的行为以防止站点无法工作。
- **如何区分**：
  - 在 HTML4.01 标准中，浏览器解析时到底使用标准模式还是兼容模式，与网页中的 DTD 直接相关，因为 HTML 4.01 基于 SGML，DTD 规定了标记语言的规则，这样浏览器才能正确地呈现。 且有三种
  - HTML5 不基于 SGML，因此不需要对 DTD 进行引用。只需要在顶部声明 `<!DOCTYPE html>`
- **目的**：防止呈现文档时浏览器切换到 “兼容模式”。
- [怪异模式和标准模式](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Quirks_Mode_and_Standards_Mode)
- [怪异模式（Quirks Mode）对 HTML 页面的影响](https://www.ibm.com/developerworks/cn/web/1310_shatao_quirks/)

## 页面导入样式时，使用 link 和 @import 有什么区别？

- `link` 属于 XHTML 标签，除了加载 CSS 外，还能用于定义 RSS，定义 rel 连接属性等作用；而 `@import` 是 CSS 提供的，只能用于加载 CSS；
- 页面被加载的时，`link` 会同时被加载，而 `@import` 引用的 CSS 会等到页面被加载完再加载；
- `@import` 是 CSS2.1 提出的，只在 IE5 以上才能被识别，而 `link` 是 XHTML 标签，无兼容问题；
- `link` 方式的样式的权重高于 `@import` 的权重。

## HTML 中 form 里 action 方法的 Get 和 Post 有什么区别？

- Get：Form 的默认方法。
- Get 是用来从服务器上获得数据。Post 是用来向服务器上传递数据
- Get 将表单中数据的按照 name=value 的形式，添加到 action 所指向的 URL 后面，并且两者使用 "?" 连接，而各个变量之间使用 "&" 连接。Post 是将表单中的数据放在 form 的数据体中，按照变量和值相对应的方式，传递到 action 所指向 URL
- Get 是不安全的，因为在传输过程，数据被放在请求的 URL 中。Post 的所有操作对用户来说都是不可见的，其放置 request body 中
- Get 传输的数据量小，这主要是因为受 URL 长度限制。Post 可以传输大量的数据，所以在上传文件只能使用 Post
- Get 限制 Form 表单的数据集的值必须为 ASCII 字符。Post 支持整个 ISO10646 字符集
- Get 请求浏览器会主动 cache。Post 支持不会
- Get 请求参数会被完整保留在浏览历史记录中。Post 中的参数不会被保留。
- GET 和 POST 本质上就是 TCP 链接，并无差别。但是由于 HTTP 的规定和浏览器/服务器的限制，导致他们在应用过程中体现出一些不同。
- GET 产生一个 TCP 数据包；POST 产生两个 TCP 数据包。

## Meta viewport 原理是什么？

- meta viewport 标签的作用是让当前 viewport 的宽度等于设备的宽度，同时当设置 `user-scalbale="no"` 时不允许用户进行手动缩放
- viewport 的原理：移动端浏览器通常都会在一个比移动端屏幕更宽的虚拟窗口中渲染页面，这个虚拟窗口就是 viewport；目的是正常展示没有做移动端适配的网页，让他们完整的展示给用户；
- **viewport 属性值**

| 属性          | 描述                                   |
| ------------- | -------------------------------------- |
| width         | 设备的虚拟视口的宽度。                 |
| height        | 设备的虚拟视口的高度。                 |
| device-width  | 设备屏幕的物理宽度。                   |
| device-height | 设备屏幕的物理高度。                   |
| initial-scale | 访问页面时的初始缩放。1.0 无法缩放。   |
| user-scalable | 允许设备放大和缩小，值为 yes 或 no。   |
| minimum-scale | 允许用户的最小缩放值，1.0 表无法缩放。 |
| maximum-scale | 允许用户的最大缩放值，1.0 表无法缩放。 |

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
```

## Meta 的目的是什么？

- `meta` 元素可用于包含描述 HTML 文档属性的名称/值对，如作者，字符编号，关键字列表，文档作者等信息

```html
<!DOCTYPE html>
<html>
  <head>
    <!-- 推荐 Meta Tags -->
    <meta charset="utf-8" />
    <meta name="language" content="english" />
    <meta http-equiv="content-type" content="text/html" />
    <meta name="author" content="Author Name" />
    <meta name="designer" content="Designer Name" />
    <meta name="publisher" content="Publisher Name" />
    <meta name="no-email-collection" content="name@email.com" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />

    <!-- 搜索引擎优化 Meta Tags -->
    <meta name="description" content="Project Description" />
    <meta
      name="keywords"
      content="Software Engineer,Product Manager,Project Manager,Data Scientist"
    />
    <meta name="robots" content="index,follow" />
    <meta name="revisit-after" content="7 days" />
    <meta name="distribution" content="web" />
    <meta name="robots" content="noodp" />

    <!-- 可选 Meta Tags-->
    <meta name="distribution" content="web" />
    <meta name="web_author" content="" />
    <meta name="rating" content="" />
    <meta name="subject" content="Personal" />
    <meta name="title" content=" - Official Website." />
    <meta name="copyright" content="Copyright 2020" />
    <meta name="reply-to" content="" />
    <meta name="abstract" content="" />
    <meta name="city" content="Bangalore" />
    <meta name="country" content="INDIA" />
    <meta name="distribution" content="" />
    <meta name="classification" content="" />

    <!-- 移动设备上 HTML 页面的 Meta Tgas -->
    <meta name="format-detection" content="telephone=yes" />
    <meta name="HandheldFriendly" content="true" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta name="apple-mobile-web-app-capable" content="yes" />

    <!-- http-equiv Tags -->
    <meta http-equiv="Content-Style-Type" content="text/css" />
    <meta http-equiv="Content-Script-Type" content="text/javascript" />

    <title>HTML5 Meta Tags</title>
  </head>
  <body>
    ...
  </body>
</html>
```

- Meta Refresh

```html
<!-- 在5秒钟内重定向到提供的 URL。设置为0可立即重定向 -->
<meta
  http-equiv="refresh"
  content="3;url=https://juejin.cn/user/96412754251390"
/>
```

- Note：如果你的网站不是专门为响应而设计的，并且在这个尺寸下工作得很好，不要使用响应元标签，因为它会让体验变得更糟。
- [Stop using the viewport meta tag (until you know how to use it)](http://www.javierusobiaga.com/blog/stop-using-the-viewport-tag-until-you-know-ho/)
- [metatags](https://www.metatags.org/all-meta-tags-overview/)

## 什么是替换元素与非替换元素

- **替换元素**：就是浏览器根据其标签的元素属性来判断显示具体的内容的元素，且元素一般拥有固定的尺寸（宽高或宽高比）。
  - 在 html 中像这样的元素有 `img`，`input`，`textarea`，`select`，`object`，这些都是替换元素，这些元素都没有实际的内容。
- **非替换元素**：html 中大多数都是非替换元素，他们直接将内容告诉浏览器，直接显示出来。
  - 如：p 标签，浏览器会直接显示 p 标签里的内容。
- tips：替换元素和非替换元素不仅是在行内元素中有，块级元素也有替换和非替换之分，比如嵌入的文档 `iframe`，`audio`，`canvas` 在某些情况下也为替换元素。

## 块级元素和行内元素的区别？

- HTML4.01 中，元素被分成两大类：[inlink（行内元素）](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Inline_elements) 与 [block（块级元素）](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Block-level_elements)。区别如下：

| 块级元素                       | 行内元素                                                                                       |
| ------------------------------ | ---------------------------------------------------------------------------------------------- |
| 独占一行                       | 不独占一行                                                                                     |
| 可以设置宽高（盒模型）         | 不可以设置宽高，宽高由元素内部的内容决定，`padding` 和 `margin` 的 `top/bottom` 不会对元素生效 |
| 可以包含行内元素和其他块级元素 | 行内元素只能包含文本和其他行内元素。                                                           |

- 默认情况下是这样，但可以利用 `display` 来修改其为块级还是行内

## 块级元素和行内元素分别有哪些？ 空（void）元素有哪些？

- CSS 规范规定，每个元素都有 `display` 属性，每个元素都有默认的 `display` 值。例如：
  - div 默认`display` 属性值为 `block`，为块级元素；
  - span 默认 `display` 属性值为 `inline`，为行内元素。
- 块级元素：
  - `<h1>-<h6>`、`<p>`、`<div>`、`<ul>`、`<ol>`、`<form>`、`<table>`、`<address>`、`<blockquote>`、`<center>`、`<dir>`、`<dl>`、`<fieldset>`、`<hr>`、`<menu>`、`<noscript>`、`<pre>`、`<noframes>`、`<isindex>`
  - 当元素的 `display` 属性为 `block`、`list-item` 或 `table` 时，该元素将成为 “块级元素”。
- 行内元素：
  - `<a>`、`<img>`、`<input>`、`<span>`、`<textarea>`、`<label>`、`<select>`、`<abbr>`、`<acronym>`、`<b>`、`<bdo>`、`<big>`、`<br>`、`<cite>`、`<code>`、`<dfn>`、`<em>`、`<font>`、`<i>`、`<kbd>`、`<q>`、`<s>`、`<samp>`、`<small>`、`<strike>`、`<strong>`、`<sub>`、`<sup>`、`<tt>`、`<u>`
  - 当元素的 `display` 属性为 `inline`、`inline-block` 或 `inline-table` 时，该元素将成为 “行内元素”
- 常见的空元素：标签内没有内容的 HTML 标签被称为空元素。
  - `<br>`、`<hr>`、`<img>`、`<input>`、`<link>`、`<meta>`
- 不常见的空元素：`<area>`、`<base>`、`<col>`、`<command>`、`<embed>`、`<keygen>`、`<param>`、`<source>` `<track>`、`<wbr>`

## 什么是可选标签？为什么要使用它？

- 在 HTML 中，某些元素具有可选标签。实际上，即使元素本身是必需的，也可以从 HTML 文档中完全删除某些元素的开始和结束标签。

- `p`，`li`，`td`，`tr`，`th`，`html`，`head`，`body` 等。如：

```html
<p>Paragraph one.</p>
<p>Paragraph two.</p>
<p>Paragraph three.</p>
```

- 您不必提供 `</p>` 结束标签。浏览器会检测到它需要它们，并且无论如何都会正确显示在 DOM 中。但这可能带给你编写上的困难！
- 您可以节省一些字节并减少需要在 html 文件中下载的字节。

> 为了便于阅读，当您的标签内有内容/文本时，带上结束标签。

## 简述一下 src 与 href 的区别？

- src 用于引用资源，替换当前元素；href 用于在当前文档和引用资源之间确立联系。
- href（hyperReference）即超文本引用：浏览器遇到并行下载资源，不阻塞页面解析，与 link 引入 css 一样，浏览器并行下载 css 不阻塞页面解析
  - href 是指向网络资源所在位置，建立和当前元素（锚点）或当前文档（链接）之间的链接，用于超链接。
- src （resource）即资源：当浏览器遇到 src 时，会暂停页面解析，直到该资源下载或执行完毕，这也是 script 标签之所以放底部的原因
  - src 是指向外部资源的位置，指向的内容将会嵌入到文档中当前标签所在位置；
  - 在请求 src 资源时会将其指向的资源下载并应用到文档内，例如 js 脚本，img 图片和 frame 等元素。
  - 当浏览器解析到该元素时，会暂停其他资源的下载和处理，直到将该资源加载、编译、执行完毕，图片和框架等元素也如此，类似于将所指向资源嵌入当前标签内。这也是为什么将 js 脚本放在底部而不是头部。

## img 上 title 与 alt

- `title` 是鼠标放在图片上面时显示的文字，`title` 是对图片的描述和进一步的说明。
- `alt` 定义了图像的备用文本描述。
- tips：浏览器并非总是会显示图像。当有下列情况时，`alt` 属性可以为图像提供替代的信息：
  - 非可视化浏览器（Non-visual browsers）（比如有视力障碍的人使用的音频浏览器）
  - 用户选择不显示图像（比如为了节省带宽，或出于隐私等考虑不加载包括图片在内的第三方资源文件）
  - 图像文件无效，或是使用了[不支持的格式](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/img#Supported_image_formats)
  - 浏览器禁用图像等
  - tips：建议尽可能地通过 `alt` 属性提供一些有用的信息。

```html
<!-- × -->
<img src="wenhao.jpg" alt="图片" />

<!-- √ -->
<img src="wenhao.jpg" alt="满脸问号的男人" />
```

### **图像上 alt 属性的用途是什么？**

- 如果用户无法查看图像，`alt` 属性将为图像提供可选信息。`alt` 属性应该用来描述任何图像，除了那些仅用于装饰目的的图像，在这种情况下，应该将其留空。
- 装饰性图像应具有空 `alt` 属性。
- 网络爬虫使用 `alt` 标签来理解图像内容，因此它们被认为对搜索引擎优化（SEO）很重要。
- 在 `alt` 标签的末尾增加可访问性。

## 为什么在 img 标签中使用 srcset 属性？请描述浏览器遇到该属性后的处理过程。

- 为了设计响应式图片。我们可以使用两个新的属性 `srcset` 和 `sizes` 来提供更多额外的资源图像和提示，帮助浏览器选择正确的一个资源。
- **srcset**：定义了我们允许浏览器选择的图像集，以及每个图像的大小。
- **sizes**：定义了一组媒体条件（例如屏幕宽度）并且指明当某些媒体条件为真时，什么样的图片尺寸是最佳选择。
- 处理过程：
  - 查看设备宽度
  - 检查 sizes 列表中哪个媒体条件是第一个为真
  - 查看给予该媒体查询的槽大小
  - 加载 srcset 列表中引用的最接近所选的槽大小的图像

```html
<img
  srcset="small.jpg 500w, medium.jpg 1000w, large.jpg 2000w"
  src="..."
  alt=""
/>
```

- [响应式图像：如果您只是在更改分辨率，请使用 srcset。](https://css-tricks.com/responsive-images-youre-just-changing-resolutions-use-srcset/)
- [响应式图像教程](http://www.ruanyifeng.com/blog/2019/06/responsive-images.html)

## noscript 标签的作用

`<noscript>` 标签用来定义在脚本未被执行时的替代内容（文本）。

`<noscript>` 标签中的内容只有在下列情况下才会显示出来：

- 浏览器不支持脚本
- 浏览器支持脚本，但脚本被禁用

```html
<!-- 给予用户友好的提示! -->
<noscript>您的浏览器不支持 JavaScript！</noscript>
```

## label 的作用是什么？是怎么用的？

`label` 标签定义表单控件的关系，当用户选择该标签时，浏览器会自动将焦点转到和标签相关的表单控件上。

有两种 HTML 原生方法可以将 `label` 元素与 `input` 元素连接起来。一种是 `id` 绑定，一种是嵌套

```html
<label for="select">爱我</label>
<input type="radio" id="select" name="love" value="love" />

<!-- 用 label 包装 input 元素 -->
<label>
  恨我
  <input type="radio" name="hate" value="hate" />
</label>
```

> **注意**：点击 `label` 的时候，事件冒泡一次，`label` 会触发关联 `input` 的 `click` 事件，导致事件再次触发事件。

## title 与 h1 的区别、b 与 strong 的区别、i 与 em 的区别？

### `<title>` 与 `<h1>` 的区别

- `<title>` 用于网站信息标题，一个网站可以有多个 `title`，seo 权重高于 `h1`；
- `<h1>` 概括的是文章主题，一个页面最好只用一个 `h1`，seo 权重低于 `title`。

### `<b>` 与 `<strong>` 的区别

- `<b>` 为了加粗而加粗
- `<strong>` 为了标明重点内容而加粗，有语气加强的含义

### `<i>` 与 `<em>` 的区别

`<i>` 为了斜体而斜体，`<em>` 为了标明重点而斜体，且对于搜索引擎来说 `<strong>` 和 `<em>` 比 `<b>` 和 `<i>` 要重视的多

## `rel="noopener"` 应在什么场景下使用，为什么？

- `rel="noopener"` 是 `<a>` 标签的一个属性。他可以禁止打开的新页面中使用 `window.opener` 属性，这样一来打开的新页面就不能通过 `window.opener` 去操作你的页面。
- 新页面可以通过 `window.opener.location = newURL` 将你的页面导航至任何网址。

```html
<!-- home.html -->
<a href="./home.html" target="_blank">home</a>
<!-- <a href="./home.html" target="_blank" rel="noopener">home</a> -->

<!-- details.html -->
<h1>点关注 不迷路！</h1>
<script>
  window.opener.location = 'https://juejin.cn/user/96412754251390'
</script>
```

- 如果您正在使用带有 `target ="_ blank"` 的外部链接，则您的链接应具有 `rel="noopener"` 属性，以防止标签被挪用。如果您需要支持旧版本的 Firefox，请使用 `rel="noopener noreferrer"`
- 总结：`rel="noopener"` 应用于超链接，防止打开的链接操纵源页面

## 为什么最好把 link 标签放在 head 之间？

- 把 `<link>` 标签放在 `<head></head>` 之间是规范要求的内容。此外，这种做法可以让页面逐步呈现，提高了用户体验。
- 将样式表放在文档底部附近，会使许多浏览器（包括 Internet Explorer）不能逐步呈现页面。
- 一些浏览器会阻止渲染，以避免在页面样式发生变化时，重新绘制页面中的元素。
- 这种做法可以防止呈现给用户空白的页面或没有样式的内容。

## 为什么最好把 JS 的 script 标签放在 body 之前，有例外情况吗？

- 脚本在下载和执行期间会阻止 HTML 解析。把 `<script>` 标签放在底部，保证 HTML 首先完成解析，将页面尽早呈现给用户。
- 例外情况：当你的脚本里包含 `document.write()` 时。
  - 但是现在，`document.write()` 不推荐使用。
  - 同时，将 `<script>` 标签放在底部，意味着浏览器不能开始下载脚本，直到整个文档（document）被解析。对此比较好的做法是，`<script>` 使用 `defer` 属性，放在 `<head>` 中。

## HTML 全局属性（global attribute）有哪些

- `accesskey`：设置快捷键，提供快速访问元素
- `class`：为元素设置类标识，多个类名用空格分开，CSS 和 JavaScript 可通过 class 属性获取元素
- `contenteditable`：指定元素内容是否可编辑
- `contextmenu`：自定义鼠标右键弹出菜单内容
- `data-*`：为元素增加自定义属性
- `dir`：设置元素文本方向
- `draggable`：设置元素是否可拖拽
- `dropzone`：设置元素拖放类型： copy，move，link
- `hidden`：表示一个元素是否与文档。样式上会导致元素不显示，但是不能用这个属性实现样式效果
- `id`：元素 id，文档内唯一
- `lang`：元素内容的的语言
- `spellcheck`：是否启动拼写和语法检查
- `style`：行内 CSS 样式
- `tabindex`：设置元素可以获得焦点，通过 tab 可以导航
- `title`：元素相关的建议信息
- `translate`：元素和子孙节点内容是否需要本地化

## 请描述下 SEO 中的 TDK？

在 SEO 中，所谓的 TDK 其实就是 `title`、`description`、`keywords` 这三个标签

- `title` 标题标签
- `description` 描述标签
- `keywords` 关键词标签

## 你有使用过什么模板引擎？为什么使用它？

- 常用模板引擎：[Pug](https://pugjs.org/)（以前叫 Jade）、 [Haml](http://haml.info/docs.html)、[Handlebars](http://handlebarsjs.com/)、[art-template](https://aui.github.io/art-template/) 等
- 这些模版语言大多是相似的，都提供了用于展示数据的内容替换和过滤器的功能
- 大部分模版引擎都支持自定义过滤器，以展示自定义格式的内容。

```jade
!!! 5
html
  head
    title = HelloWorld
  body
    h1使用Jade创建HelloWorld网页
```

编译为

```html
<!DOCTYPE html>
<html>
  <head>
    <title>HelloWorld</title>
  </head>
  <body>
    <h1>使用Jade创建HelloWorld网页</h1>
  </body>
</html>
```

- 它帮助我们用更少的代码更快地编写 HTML 代码。

## iframe 是什么？有什么优缺点？

- HTML 内联框架元素 [`<iframe>`](<[iframe](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/iframe)>) 表示嵌入的[浏览上下文](https://developer.mozilla.org/en-US/docs/Glossary/browsing_context)。它能够将另一个 HTML 页面嵌入到当前页面中。
- iframe 是用来在网页中插入第三方页面，早期的页面使用 iframe 主要是用于导航栏这种很多页面都相同的部分，这样在切换页面的时候避免重复下载。

> **Tips**：可以将提示文字放在 `<iframe></iframe>` 之间，来提示某些不支持 iframe 的浏览器

### 优点

- 便于修改，模拟分离，像一些信息管理系统会用到。
- iframe 能够原封不动的把嵌入的网页展现出来。
- 如果有多个网页引用 iframe，那么你只需要修改 iframe 的内容，就可以实现调用的每一个页面内容的更改，方便快捷。
- 网页如果为了统一风格，头部和版本都是一样的，就可以写成一个页面，用 iframe 来嵌套，可以增加代码的可重用。
- 并行加载脚本
- security sandbox（安全沙盒）
- 解决加载缓慢的第三方内容。如：图标和广告等的加载问题。

### 缺点

- 没有语意。搜索引擎无法解读这种页面，不利于 SEO
- 会产生很多页面，不容易管理。
- 即使内容为空，加载也需要时间。
- iframe 的创建比一般的 DOM 元素慢了 1-2 个数量级
- 很多的移动设备（PDA 手机）无法完全显示框架，设备兼容性差。
- iframe 框架页面会增加服务器的 http 请求，对于大型网站是不可取的。
- iframe 会阻塞主页面的 onload 事件。
- iframe 和主页面共享连接池，而浏览器对相同域的连接有限制，所以会影响页面的并行加载。
- 如果需要使用 iframe，最好通过 javascript 动态给 iframe 添加 src 属性值，这样可以绕开以上两个问题
- iframe 框架结构有时会让人感到迷惑，如果框架个数多的话，可能会出现上下、左右滚动条，会分散访问者的注意力，用户体验度差。

> **Tips**：除非特殊需要，一般不推荐使用。目前 iframe 的优点完全可以使用 AJAX 实现，因此已经没有必要使用 iframe 了。

## div+css 的布局较 table 布局有什么优点？

分离方便、改版快、清晰简洁、seo

- 表现与结构相分离。
- 改版的时候更方便，只要改 css 文件。
- 页面加载速度更快、结构化清晰、页面显示简洁。
- 易于优化（seo）搜索引擎更友好，排名更容易靠前。

## 很多网站不常用 table，iframe 这两个元素，知道原因吗？

因为浏览器页面渲染的时候是从上至下的，而 `table` 和 `iframe` 这两种元素会改变这样渲染规则，他们需要等待自己元素内的内容加载完才整体渲染。用户体验会很不友好。

- 还有一些其他的影响：详细参考[为什么我们不建议用 Table 布局](https://www.html5tricks.com/why-not-table-layout.html)，`iframe` 的话本文有所提及，可以翻阅查找

## HTML 中的 lang 属性有什么作用？

- 通过在 `css:lang()` 伪类拼写和语法检查器中使用页面样式来帮助搜索引擎进行语言检测

```css
:lang(zh) {
  font-size: 1.5em;
}
```

- [HTML 的 lang 属性的作用](https://www.cnblogs.com/wangmeijian/p/6815375.html)
- [HTML 中 html 元素的 lang 属性的说明](https://blog.csdn.net/mirro81/article/details/75213031)
- [网页头部的声明应该是用 lang="zh" 还是 lang="zh-cn"？](https://www.zhihu.com/question/20797118/answer/16809331)

## 什么 `enctype='multipart/form-data'` 意思？

`enctype` 属性指定将表单数据提交到服务器时应如何编码。

## HTML5

HTML5 是 HTML（超文本标记语言）最新的修订版本，由万维网联盟（W3C）于 2014 年 10 月完成标准制定。目标是取代 1999 年所制定的 HTML 4.01 和 XHTML 1.0 标准。它是一种为万维网构建和显示内容的语言，万维网是互联网的核心技术。

### HTML5 向后兼容旧浏览器吗？

HTML5 被设计成尽可能向后兼容现有的 web 浏览器

### HTML5 为什么只需要写 `<!DOCTYPE html>`？

- HTML5 不基于 SGML，因此不需要对 DTD 进行引用，但是需要 DOCTYPE 来规范浏览 器的行为（让浏览器按照它们应该的方式来运行）
- HTML4.01 基于 SGML，所以需要对 DTD 进行引用，才能告知浏览器文档所使用的文档类型。
- 其中，SGML 是标准通用标记语言，简单的说，就是比 HTML，XML 更老的标准，这两者都是由 SGML 发展而来的。但是，HTML5 不是。

### HTML5 文档类型和字符集是？

- HTML5 文档类型：`<!DOCTYPE html>`
- HTML5 字符集编码：`<meta charset="UTF-8" />`

### HTML5 有哪些新特性

- 在 HTML5 中，DOM 拓展了三种选择器 `document.querySelector`、`document.querySelectorAll`、`matchesSelector()`
- 拖拽释放（Drag and drop）API
- 绘画 `canvas`，支持内联 SVG。支持 MathML
- 媒体播放的 `video` 和 `audio` 元素
- 本地离线存储 `localStorage` 长期存储数据，浏览器关闭后数据不丢失；`sessionStorage` 的数据在浏览器关闭后自动删除
- 更好的实践 web 语义化，比如 `article`、`footer`、`header`、`nav`、`section` 等语义标签
- 表单控件：`calendar`、`date`、`time`、`email`、`url` 等
- 新增表单元素：`datalist`、`keygen`、`output`
- 新的技术：多线程编程的 `webWorker`， 全双工通信协议 `WebSocket` 和地理定位 `Geolocation`

### 什么是 data-\* 属性？

- 自定义数据属性以 data 开始，并将根据您的需求进行命名
- 您可以使用 JavaScript 获得这些属性的值

- 在 JavaScript 框架变得流行之前，前端开发者经常使用 `data-*` 属性，把额外数据存储在 DOM 自身中，而当时没有其他 Hack 手段（比如使用非标准属性或 DOM 上额外属性）。它用于存储页面或应用程序专用的自定义数据，对于这些数据，没有更合适的属性或元素。
- 而现在，不鼓励使用 `data-*` 属性。原因：
  - 用户可以通过在浏览器中利用检查元素，轻松地修改属性值，借此修改数据
  - 数据模型最好存储在 JavaScript 本身中，并利用框架提供的数据绑定，使之与 DOM 保持更新

### HTML5 中不推荐使用哪些 HTML 标签？

- `<acronym>`：建议用 `<abbr>`
- `<applet>`：建议用 `<object>`
- `<basefont>`：建议用 `<font>`
- `<bgsound>`：建议用 `<audio>`
- `<b>`：不推荐使用，建议用 `font-weight` 代替
- `<big>`：不推荐使用，建议用 `font-size` 代替
- `<blink>`：建议采用 `animation` 代替
- `<center>`：不推荐使用，建议用 `text-align: center` 代替
- `<dir>`：建议根据语义采用 `<ul>`、`<ol>` 或者 `<dl>`
- `<font>`：不推荐使用，建议用 CSS 代替，以便更好控制。
- `<frame>`：不推荐使用，建议用 `<iframe>` 代替。但 `<iframe>` 现在又可以用 ajax 来代替
- `<frameset>`：不推荐使用
- `<hgroup>`：不推荐使用
- `<isindex>`：建议用 `<input>`
- `<listing>`：建议用 `<pre>` 或使用语义更好的 `<code>`
- `<marquee>`：不推荐使用
- `<multicol>`：建议用 `<input>`
- `<nobr>`：不推荐使用，建议用 `white-space` 代替
- `<noframes>`：不推荐使用，建议用 帧 CSS 代替
- `<plaintext>`：建议用 `<pre>` 或使用语义更好的 `<code>`
- `<u>`：不推荐使用，建议用 帧 `font-style` 代替
- `<spacer>`：不推荐使用
- `<strike>`：建议用语义更好的 `<del>` 或 `<s>`
- `<tt>`：建议用 `<code>` 或 `<span>`
- `<xmp>`：建议用带有 CSS 的 `<pre>` 或使用语义更好的 `<code>`
- [HTML5 Differences from HTML4](http://www.w3.org/TR/html5-diff)
- [HTML 元素参考](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element)

### 你如何理解 Web 的语义化?

Web 语义化是指通过 HTML 标签表示页面包含的信息，包含了 HTML 标签的语义化和 CSS 命名的语义化。

- HTML 标签的语义化 — 通过使用包含语义的标签（如 `h1-h6`）恰当地表示文档结构
- CSS 命名的语义化 — 为 HTML 标签添加有意义的 `class`，`id` 补充未表达的语义，如 [Microformat](http://en.wikipedia.org/wiki/Microformats) 通过添加符合规则的 `class` 描述信息

#### HTML 为什么需要结构语义化

- 便于团队开发和维护。
- 在没有 CSS 样式的情况下，能让页面呈现清晰的结构。
- 屏幕阅读器（如果访客有视障）会完全根据你的标签来 "读" 你的网页。
- 搜索引擎的爬虫依赖于标签来确定上下文和各个关键字的权重，利于 SEO。

> 它用于改进文档的自动化处理。自动处理发生的频率比你意识到的要高，搜索引擎中的每个网站排名都是从所有网站的自动处理中派生出来的。

```html
<!-- 机器：这种结构看起来可能是导航元素？ -->
<div class="some-meaningless-class"><ul><li><a href="internal_link">...</div>

<!-- 机器: 这是导航元素！ -->
<nav class="some-meaningless-class"><ul><li><a>...</nav>
```

#### 什么是语义和非语义元素？

- **语义元素**：对浏览器和开发人员都清楚地描述了其含义。例如：`<form>`，`<table>`，`<figcaption>`，`<figure>`，`<header>`，`<footer>`，`<main>`，`<aside>`，`<article>`，`<section>`，`<nav>`，`<mark>`，`<h1~h6>`，`<details>`，`<summary>`，`<time>` 明确规定其含义。
- **非语义元素**：`<div>`，`<span>` 等不包含任何含义。

#### 简述 HTML5 一些语义的用法

- `<header>` 用于包含有关页面某个部分的介绍性和导航信息。这可以包括章节标题、作者姓名、出版时间和日期、目录或其他导航信息。
- `<article>` 是用来存放一个自成体系的作文，在逻辑上可以在页面之外独立地重新创建，而不会失去它的意义。个人博客文章或新闻故事就是很好的例子。
- `<section>` 是一个灵活的容器，用于保存共享公共信息主题或目的的内容。
- `<footer>` 用于保存应该出现在内容节末尾的信息，并包含有关该节的附加信息。作者姓名、版权信息和相关链接是此类内容的典型示例。
- `<main>` 元素表示 `body` 文档的主要内容。主要内容区域由与文档的中心主题或应用程序的中心功能直接相关或扩展的内容组成。
- 查看 [Semantic in HTML5](https://htmlreference.io/semantic/) 以了解其他的语义标签。
- [Why use an HTML5 semantic tag instead of div?](https://stackoverflow.com/questions/17272019/why-use-an-html5-semantic-tag-instead-of-div)
- [Should I use <i> tag for icons instead of <span>? [closed]](https://stackoverflow.com/questions/11135261/should-i-use-i-tag-for-icons-instead-of-span)
- [What is the difference between <section> and <div>?](https://stackoverflow.com/questions/6939864/what-is-the-difference-between-section-and-div)
- [How To Write Semantic HTML](https://hackernoon.com/how-to-write-semantic-html-dkq3ulo)

### Canvas 和 SVG

- [Canvas](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/canvas) 和 [SVG](https://developer.mozilla.org/zh-CN/docs/Glossary/SVG) 都允许您在浏览器中创建图形，但是它们在根本上是不同的。

#### SVG

> SVG 意为可缩放矢量图形（Scalable Vector Graphics）是一种基于 [XML](https://developer.mozilla.org/en-US/docs/Glossary/XML) 的图像格式，用于为 web 定义基于向量的二维图形。与光栅图像（例如 .jpg、.gif、.png 等）不同，矢量图像可以在不损失图像质量的情况下进行任何程度的放大或缩小。

```html
<!-- SVG 是矢量和声明性的 -->
<svg viewBox="0 0 200 200">
  <circle cx="10" cy="10" r="10" />
</svg>
```

#### Canvas

> Canvas 是一个 HTML5 元素，用于在网页上绘制图形。它是一个带有 "立即模式" 图形应用程序编程接口（API）的位图，用于在其上绘图。`<canvas>` 元素只是图形的容器。为了绘制图形，你应该使用 js 画布在绘制路径、方框、圆、文本和添加图像时有几种策略。

- HTML `<canvas>` 元素提供了一个空白绘图区域，可通过 JavaScript API（[Canvas](https://developer.mozilla.org/zh-CN/docs/Web/API/Canvas_API) API 或 [WebGL](https://developer.mozilla.org/zh-CN/docs/Web/API/WebGL_API) API）绘制图形及图形动画
  - 通过 JavaScript 来绘制 2D 图形
  - 是逐像素进行渲染的
  - 其位置发生改变，会重新进行绘制

```html
<canvas id="myCanvas" width="578" height="200"></canvas>
<script>
  var canvas = document.getElementById('myCanvas')
  var context = canvas.getContext('2d')
  var centerX = canvas.width / 2
  var centerY = canvas.height / 2
  var radius = 70

  context.beginPath()
  context.arc(centerX, centerY, radius, 0, 2 * Math.PI, false)
  context.fillStyle = 'green'
  context.fill()
</script>
```

- Canvas 的一些预期用途包括构建图形、动画、游戏和图像合成。
- Canvas 元素用于在网页上绘制图形，该元素标签强大之处在于可以直接在 HTML 上进行图形操作

#### Canvas 和 SVG 的区别

| Canvas                                             | SVG                                                      |
| -------------------------------------------------- | -------------------------------------------------------- |
| 基于栅格（由像素组成）                             | 基于矢量（由形状组成），非常适合 UI/UX 动画              |
| 依赖分辨率                                         | 不依赖分辨率                                             |
| 不支持事件处理器                                   | 支持事件处理器                                           |
| 文本渲染能力差                                     | 良好的文字渲染功能                                       |
| 使用更多的对象或更小的曲面或两者都提供更好的性能   | 复杂度高会减慢渲染速度（任何过度使用 DOM 的应用都不快）  |
| 最适合图像密集型的游戏，其中的许多对象会被频繁重绘 | 不适合游戏应用                                           |
| 仅通过脚本修改                                     | 通过脚本和 CSS 修改                                      |
| 能够以 .png 或 .jpg 格式保存结果图像               | 多个图形元素，成为页面 DOM 树的一部分                    |
| 可伸缩性差。不适合以较高分辨率打印。可能发生像素化 | 更好的可扩展性。可以任何分辨率高质量打印。不会发生像素化 |

#### 100 \* 100 的 canvas 占多少内存？

- [100\*100 的 canvas 占多少内存](https://juejin.cn/post/6844903704139661326)

#### 您如何为您的网站选择 svg 或 canvas？

- 如果你知道你需要矢量艺术，则选择 SVG。与 JPG 之类的栅格图形相比，矢量艺术在视觉上是清晰的，并且文件大小通常较小。
- 像一个小的平面颜色图标，这显然是 SVG 的领域。
- 像互动游戏，那显然是 Canvas。

#### 参考

- [Canvas API](https://www.bookstack.cn/books/webapi-tutorial)
- [SVG 图像](https://www.bookstack.cn/read/webapi-tutorial/docs-svg.md)

- [How to Choose Between Canvas and SVG](https://www.sitepoint.com/how-to-choose-between-canvas-and-svg/)
- [何时使用 SVG 与何时使用 canvas](https://css-tricks.com/when-to-use-svg-vs-when-to-use-canvas/)

### 新标签

#### 新语义和结构元素

- `canvas` 标签定义图形，比如图表和其他图像。该标签基于 JavaScript 的绘图 API

```html
<canvas width="300" height="300"> 抱歉，您的浏览器不支持canvas元素 </canvas>
```

- `figure` 是当您想要显示带有标题的图像时经常使用的语义标签。经常与 `img` 和 `figcaption` 标签配合使用。
- `figcaption` 标签包含标题文本。

```html
<style>
  figure {
    border: thin #c0c0c0 solid;
    display: flex;
    flex-flow: column;
    padding: 5px;
    max-width: 220px;
    margin: auto;
  }

  img {
    max-width: 220px;
    max-height: 150px;
  }

  figcaption {
    background-color: #222;
    color: #fff;
    font: italic smaller sans-serif;
    padding: 3px;
    text-align: center;
  }
</style>

<figure>
  <img src="avatar.jpg" alt="D.O" />
  <figcaption>An elephant at sunset</figcaption>
</figure>
```

- `summary` 利用了一个 `details` 元素的一个内容的摘要，标题或图例。
- `details` 可创建一个挂件，仅在被切换成展开状态时，它才会显示内含的信息。
  - 标签用于描述文档或文档某个部分的细节。
  - 给他来点动画效果：[如何对 Details 元素进行动画处理](https://css-tricks.com/how-to-animate-the-details-element/)

```html
<style>
  details {
    border: 1px solid #aaa;
    border-radius: 4px;
    padding: 0.5em 0.5em 0;
  }
  summary {
    font-weight: bold;
    margin: -0.5em -0.5em 0;
    padding: 0.5em;
  }
  details[open] {
    padding: 0.5em;
  }
  details[open] summary {
    border-bottom: 1px solid #aaa;
    margin-bottom: 0.5em;
  }
</style>

<details>
  <summary>Details</summary>
  Something small enough to escape casual notice.
</details>
```

- `header`、`nav`、`main`、`footer`、`section`、`article`

```html
<section>
  <header></header>
  <main>
    <nav></nav>
  </main>
  <footer></footer>
</section>
<article></article>
```

- `map` 定义一个客户端图像映射。图像映射（image-map）指带有可点击区域的一幅图像
  - area 元素永远嵌套在 map 元素内部。area 元素可定义图像映射中的区域。

```html
<div>
  <img
    src="../img/cs.jpg"
    width="500"
    height="500"
    alt="pic"
    usemap="#circusmap"
  />
  <map name="circusmap">
    <area shape="rect" coords="90,18,202,186" href="https://www.baidu.com/" />
    <area
      shape="rect"
      coords="222,141,318, 256"
      href="https://www.baidu.com/"
    />
    <area
      shape="circle"
      coords="343,111,455, 267"
      href="https://www.baidu.com/"
    />
    <area shape="rect" coords="35,328,143,500" href="https://www.baidu.com/" />
  </map>
</div>
```

- `mark` 突出显示 html 中的文本。在这个标签出现之前，常使用使用 `em` 或 `strong` 赋予突出显示的内容一些语义。现在不推荐了。如果需要突出显示，请使用此标签

```html
<p><mark>Lio</mark></p>
```

默认背景颜色 `<mark>` 是黄色

```css
/* default style */
mark {
  background: yellow;
  color: black;
}
```

可以使用 CSS 自定义样式

```css
mark {
  background: red;
  color: white;
}
```

- `meter` 标签定义已知范围或分数值内的标量测量，也被称为 gauge（尺度）。它不应用于指示进度（在进度条中）。如果标记进度条，请使用 `progress` 标签。

```html
<div>
  <meter value="4" min="0" max="10">4/10</meter><br />
  <meter value="0.6">60%</meter>
</div>
```

- `progress` 标签标示任务的进度（进程）。

```html
<label for="file">Downloading progress:</label>
<progress id="file" value="32" max="100">32%</progress>
```

- `time` 定义日期或时间。Edge、Firefox 和 Safari 都支持 `<time>` 元素。
  - `datetime` 属性可用于提供机器可读的日期、时间、时区偏移量或持续时间形式。HTML 标准列出了这个属性的有效语法。几个例子：

```html
<time datetime="2011-11">November, 2011</time>
<time datetime="2009-08-29">two days ago</time>
<time datetime="2011-11-18T15:00-08:00">3pm</time>
```

- `bdi` 允许您设置一段文本，使其脱离其父元素的文本方向设置。

```html
<p dir="ltr">Lorem ipsum <bdi>dolor</bdi> sit amet.</p>
```

- `dialog` 标签定义一个对话框、确认框或窗口。

```html
<dialog open>
  <p>Greetings, one and all!</p>
</dialog>
```

#### 新多媒体元素

- `source` 定义视频源 `<video>` 和 `<audio>`
- `track` 定义文本轨道
- `video` 定义视频元素
  - HTML5 支持 mp4、webm 和 ogg 格式的视频。其中 Ogg 格式在 IE 中不受任何方式的支持
  - `src` 指定视频的来源。
  - 当不给 `video` 设置高度和宽度时，浏览器不知道视频的大小，当视频加载时，页面将发生变化或闪烁

```html
<!-- 1. 用 src 属性定义 -->
<video src="video.mp4" controls></video>

<!-- 2. 定义 source 标签 -->
<video controls>
  <source src="video.mp4" type="video/mp4" />
  你的浏览器不支持 HTML5 viedo 标签。
</video>
```

- `embed` 将外部内容嵌入文档中的指定位置。

```html
<embed src="https://juejin.cn/user/96412754251390" height="700" width="100%" />

<embed
  type="video/webm"
  src="/media/cc0-videos/flower.mp4"
  height="700"
  width="100%"
/>
```

- `audio` 定义音频内容
  - HTML5 支持 MP3、Wav 和 Ogg 格式的音频。

```html
<audio controls>
  <source src="sound.ogg" type="audio/ogg" />
  <source src="sound.mp3" type="audio/mpeg" />
  您的浏览器不支持 HTML5 audio 标签。
</audio>
```

#### 新表单元素

- `datalist`
  - `<datalist>` 标签定义选项列表。与 input 元素配合使用该元素，来定义 input 可能的值。
  - datalist 及其选项不会被显示出来，它仅仅是合法的输入值列表。
  - 使用 input 元素的 list 属性来绑定 datalist

```html
<label for="course">选择学习课程：</label>
<input list="target" name="course" id="course" />
<datalist id="target">
  <option value="HTML"></option>
  <option value="CSS"></option>
  <option value="JavaScript"></option>
  <option value="Node"></option>
  <option value="Vue"></option>
  <option value="React"></option>
  <option value="Webpack"></option>
</datalist>
```

- `keygen` 该元素有助于生成密钥和通过表单提交。

  - `keygen` 必须在表单内使用。
  - `keygen` 已经从 Web 标准中删除，请使用 JavaScript 生成密钥
  - [MDN](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/keygen)

- `output` 标签定义不同类型的输出，比如脚本的输出。

```html
<form oninput="x.value=parseInt(a.value) * parseInt(b.value)">
  0 <input type="range" id="a" value="50" /> 100 *
  <input type="number" id="b" value="1" /> =
  <output name="x" for="a b"></output>
</form>
```

#### HTML5 之前的一些元素

- `pre` 标签可定义预格式化的文本。被包围在 `<pre>` 标签中的文本通常会保留空格和换行符。而文本也会呈现为等宽字体。
- `strong` 用于指示比周围文本更重要的文本，例如警告或错误。从语义上讲，它的重要性。它显示为粗体
- `b` 与 `strong` 非常相似，因为它也显示为粗体。然而，与它不同的是，它并没有真正传达出任何重要性，它更像是一种文体而非语义。
- `em` 用于强调某个词。它显示为斜体

```html
<strong>lorem</strong>
<b>lorem</b>
<em>lorem</em>
```

- `q` 和 `blockquote`
  - `q` 引号
  - `blockquote` 块引号

```html
<q>lorem</q>
<blockquote>lorem</blockquote>
```

- `bdo` 可以更改 HTML 文本的方向。
  - `rtl`：从右到左。`ltr`：从左到右。

```html
<p><bdo dir="rtl">This text will go right to left.</bdo></p>
```

- 使用 `abbr` 标签缩写您的代码，当你传递一个标题时，它将创建一个工具提示
  - `<abbr>` 不同浏览器的默认样式有些不同。在 Chrome 和 Firefox 中，它将带有下划线，并且在悬停时将带有 `title` 传递的值的工具提示。如果您在 Safari 上打开此页面，则不会出现下划线。此外，仅当您具有 `title` 属性时才显示下划线。
  - 由于跨浏览器的差异，建议为 `<abbr>` 代码加上自定义样式。这样，您将在浏览器之间拥有一致的外观

定义术语时，可以与 `dfn` 混合使用

```html
<dfn> <abbr title="Today I learned">TIL</abbr> something awesome! </dfn>
```

指示的非缩写词并将其输出到页面上的括号中

```css
abbr[title]::after {
  content: ' (' attr(title) ')';
}
```

利用 `hover` 状态仅在点击时显示非缩写词

```css
abbr[title]:hover::after {
  content: ' (' attr(title) ')';
}
```

使用 `abbr` 标签来指示在顺序键盘导航中是可聚焦的 `tabindex="0"`，然后在聚焦时触发我们的非缩写内容。

```html
<abbr title="Today I learned" tabindex="0">TIL</abbr>
```

```css
abbr[title]:focus::after {
  content: ' (' attr(title) ')';
}
```

也可以使用一些提示工具，如 Bootstrap 的[工具提示](https://getbootstrap.com/docs/4.5/components/tooltips/)组件。

- `kbd` 和 `code`
  - `kbd`：表示用户从键盘、语音输入或任何其他文本输入设备输入的文本。
  - `code`：表示计算机代码的简短片段的文本。
  - 两者使用同样的 `monospace` 字体。但是在语义上它们是不同的。最好使用 `kbd` 代替 `code`

```html
<kbd>Ctrl</kbd> + <kbd>C</kbd> <code>Ctrl</code> + <code>C</code>
```

```css
/* Default Style */
kbd {
  font-family: monospace;
}

kbd,
code {
  border: 1px solid gray;
  border-radius: 5px;
  padding: 5px;
}
```

- `s` 和 `del` 删除线
  - `s` 当您尝试表示不再相关或不再准确的事物时，使用它。
  - `del` 当您要指示某些内容已从文档中删除时，使用它。
  - 它们都是删除线。但是，它们传达了关于内容的不同含义。

```html
<s>Lorem ipsum dolor sit amet.</s>

<!-- 常使用于商品价格折扣 -->
<span><s>$1999</s></span>
<span style="color: red;">$99</span>

<del>Lorem ipsum dolor sit amet.</del>

<!-- 常使用于待办事项清单 -->
<ul>
  <li><del>打卡</del></li>
  <li>喝杯咖啡</li>
</ul>
```

- `ins`

```html
<p>
  Lorem ipsum
  <ins>dolor sit amet consectetur adipisicing elit.</ins> Perferendis, rem.
</p>
```

- [20 HTML Elements for Better Text Semantics](https://www.sitepoint.com/20-html-elements-better-text-semantics/)

- [有哪些被低估未被广泛使用的有用的 HTML 标签？](https://www.zhihu.com/question/396745068/answer/1262953923)

### 新属性

- `contenteditable`
  - `contenteditable` 属性应用于任何 HTML 元素，它可以像 `input` 或 `<textare>` 那样工作编辑它们
  - 您可以为其添加事件监听器，监听其内容变化
  - `contenteditable` 属性值有 3 个不同的值：true、false、inherit

```html
<div contenteditable="true">
  <h1>元素可编辑</h1>
</div>
<div contenteditable="false">
  <h1>元素不可编辑</h1>
</div>
<div contenteditable="inherit">
  <h1>元素继承其父元素的可编辑状态</h1>
</div>
```

- `input`
  - `required` 必须输入内容。
  - `autofocus` 属性能够让 `button`，`input` 或 `textarea` 元素在页面加载完成时自动成为页面焦点
  - `pattern` 用正则表达式验证

```html
<!-- required -->
<input type="text" id="username1" name="username" required />

<!-- autofocus -->
<input type="text" id="username2" name="username" />

<!-- pattern -->
<input
  type="password"
  name="password"
  placeholder="请输入密码"
  pattern="^(?=.*\d)(?=.*[a-z])(?=.*[A-Z]).{6,20}$"
  required
/>
```

- **HTML5 新的表单输入类型？**
  - 新输入类型（type 13 种）：`date`、`month`、`week`、`time`、`number`、`range`、`email`、`url`、`color`、`datatime-local`、`datetime`、`search`、`tel`
  - `search`：用于搜索域，比如站点搜索或 Google 搜索，域显示为常规的文本域。
  - `url` ：用于应该包含 URL 地址的输入域在提交表单时，会自动验证 url 域的值。
  - `email`：用于应该包含 e-mail 地址的输入域，在提交表单时，会自动验证 email 域的值。
  - `datetime`：选取时间、日、月、年（UTC 时间）
  - `date`：选取日、月、年
  - `month`：选取月、年
  - `week`：选取周和年
  - `time`：选取时间（小时和分钟）
  - `datetime-local`：选取时间、日、月、年（本地时间）
  - `number`：用于应该包含数值的输入域，您还能够设定对所接受的数字的限定。
  - `range`：用于应该包含一定范围内数字值的输入域，类型显示为滑动条。
  - `color`：定义拾色器。
  - `tel`：定义用于输入电话号码的字段。
  - 其中 `datetime` 不在被推荐使用，转而使用 `datatime-local`

```html
<!-- url -->
<input type="url" />

<!-- tel -->
<input type="tel" name="tel" />

<!-- search -->
<input type="search" />

<!-- email -->
<form action="/">
  <input type="email" />
  <input type="submit" value="提交" />
</form>

<!-- date -->
<input type="date" value="2020-06-01" min="2020-01-01" max="2022-01-01" />

<!-- time -->
<input type="time" value="12:00" />

<!-- datetime -->
<input type="datetime" value="2020-09-12T23:00Z" />

<!-- week -->
<input type="week" />

<!-- month -->
<input type="month" value="2020-06-01" />

<!-- datetime-local -->
<input type="datetime-local" value="2020-09-06T23:00" />

<!-- number -->
<input type="number" name="number" min="2" max="10" value="3" />

<!-- color -->
<input type="color" onchange="showColor(event)" />

<!-- range -->
<input type="range" name="range" min="0" max="100" step="1" value="" />
```

- `hiden` 属性规定对元素进行隐藏。
  - 可以对 `hidden` 属性进行设置，使用户在满足某些条件时才能看到某个元素（比如选中复选框，等等）。然后，可使用 JavaScript 来删除 hidden 属性，使该元素变得可见。

```html
<div hidden>lorem</div>
```

- `Download`
  - 锚点标签的默认设置是导航链接，它将转到您在 `href` 属性中指定的链接
  - 添加 `download` 属性时，它将变成一个下载链接。提示您要下载的文件。下载的文件将具有与原始文件名相同的名称。但是，您也可以通过将值传递给 `download` 属性来设置自定义文件名
  - `download` 属性仅适用于同源 URL。如果的 `href` 来源与网站的来源不同，那么它将无法正常工作。换句话说，您只能下载属于该网站的文件。此属性遵循**同源策略**中的相同规则概述。

```html
<a href="../img/cs.jpg" download> 使用原始文件名下载本地文件 </a>

<a href="../img/cs.jpg" download="logo"> 使用自定义文件名下载 logo.png </a>
```

- [同源政策](https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin_policy)
- [浏览器同源政策及其规避方法](http://www.ruanyifeng.com/blog/2016/04/same-origin-policy.html)

### HTML5 的 form 如何关闭自动完成功能？

- 将 `input` 设置为 [`autocomplete=off`](https://developer.mozilla.org/en-US/docs/Web/Security/Securing_your_site/Turning_off_form_autocompletion)

```html
<!-- 不使用 autocomplete -->
<input type="email" name="email" />

<!-- 使用 autocomplete -->
<form action="/post">
  <input type="email" name="email" autocomplete="off" />
  <input type="submit" value="提交" />
</form>
```

### `<script>` 标签上的 `defer` 和 `async` 属性是什么？

- `<script>`：当遇到脚本时，HTML 停止解析，脚本被获取并立即执行。执行结束后，HTML 解析继续。
- `defer` 和 `async` 的作用：都是让脚本的下载和执行不阻塞页面的渲染

区别：

- `defer` 是推迟执行，它是等到页面渲染完毕，所有脚本下载完成，在 DOMContentLoaded 事件前按照脚本的在文档中的顺序执行；
- `async` 是立即下载并执行，加载和渲染后续文档元素的过程将和 js 脚本的加载与执行并行进行（异步）

1. 关于 `defer`，我们还要记住的是它是按照加载顺序执行脚本的
2. 标签为 `async` 的脚本并不保证按照指定它们的先后顺序执行。对它来说脚本的加载和执行是紧紧挨着的，所以不管你声明的顺序如何，只要它加载完了就会立刻执行。
3. `async` 对于应用脚本的用处不大，因为它完全不考虑依赖（哪怕是最低级的顺序执行），不过它对于那些可以不依赖任何脚本或不被任何脚本依赖的脚本来说却是非常合适的。

```html
<script src="longTime.js"></script>
<script src="longTime.js" defer></script>
<script src="longTime.js" async></script>
```

> **注意**：没有 `src` 属性的脚本（即不是内联脚本），`async` 和 `defer` 属性会被忽略。

### 如何处理 HTML5 新标签的浏览器兼容问题？如何区分 HTML 和 HTML5？

- IE6-8 支持通过 `document.createElement` 方法产生的标签，利用这一特性让这些浏览器支持 HTML5 新标签
- 使用 [html5shiv](https://github.com/aFarkas/html5shiv) 框架

- HTML5：
  - DOCTYPE 声明
  - 新增的语义元素（`<header>`、`<section>` 等）
  - 新增功能元素

### HTML5 的构成要素是什么？

- 语义：提供更准确地描述内容。
- 连接：提供新的方式与服务器通信。
- 离线和存储：允许网页在本地存储数据并有效地离线运行。
- 多媒体：在 Open Web 中，视频和音频被视为一等公民（first-class citizens）。
- 2D/3D 图形和特效：提供更多种演示选项。
- 性能和集成：提供更快的访问速度和性能更好的计算机硬件。
- 设备访问：允许使用各种输入、输出设备。
- 外观：可以开发丰富的主题。
- [HTML5](https://developer.mozilla.org/en-US/docs/Web/Guide/HTML/HTML5)

### Modernizr

- [Modernizr](https://github.com/Modernizr/Modernizr) 是一个用来检测浏览器功能支持情况的 JavaScript 库。
- 通过这个库我们可以检测不同的浏览器对于 HTML5 特性的支持情况。

```html
<article>
  <h1>通过 Modernizr 检测 HTML5 特性</h1>
</article>
<script
  crossorigin="anonymous"
  integrity="sha384-l7lIexAaQrMGAnOGdPikxQDjq8aY1MS3oqkKPS8FXlJ47UejXvEzmezjhEwHVkzm"
  src="https://lib.baomitu.com/modernizr/2010.07.06dev/modernizr.js"
></script>
<script>
  window.onload = function () {
    //通过Modernizr.对浏览器canvas功能进行检测
    if (Modernizr.canvas) {
      console.log('本浏览器支持Canvas API')
    } else {
      console.log('本浏览器不支持Canvas API')
    }
  }
</script>
```

### HTML5 存储

#### localStorage

[`localStorage`](https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage) 持久化的本地存储，除非是通过 js 删除，或者清除浏览器缓存，否则数据永远不会过期，关闭浏览器也不会丢失。

- HTML5 修订 `localStorage` 取代 `globalStorage`
- 在同源的所有标签页和窗口之间共享数据
- 数据仅保存在客户端，不与服务器进行通信，对数据的操作是同步的
- 大小限制为 5M；但实际 JavaScript 中的字符串为 UTF-16，因此每个字符需要两个字节的内存。这意味着尽管许多浏览器的限制为 5MB，但您只能存储 2.5M 个字符。
- 浏览器的支持情况：IE7 及以下版本不支持 web storage。但在 IE5-7 中有个 userData，其实也是用于本地存储。

#### sessionStorage

[`sessionStorage`](https://developer.mozilla.org/en-US/docs/Web/API/Window/sessionStorage) 存储对象存储一个会话的数据，数据会在浏览器关闭后自动删除。

- 跟 `localStorage` 一样，大小限制最多为 5M。
- 同一个会话的页面才能访问并且当会话结束后数据也会随之销毁，因此 sessionStorage 不是一种持久化的本地存储
- 与 `localStorage` 拥有统一的 API 接口；
- `localStorage` 有自己独立的存储空间；
- 对数据的操作是同步的。
- [localStorage 还能这么用](https://iammapping.com/the-other-ways-to-use-localstorage/)
- [给 localStorage 加上过期时间](https://juejin.im/post/6844903825304731656)
- [不同类型浏览器存储的入门](https://css-tricks.com/a-primer-on-the-different-types-of-browser-storage/)
- [前端存储除了 localStorage 还有啥](https://juejin.cn/post/6844904192549584903#heading-12)

#### cookie、sessionStorage 和 localStorage 的区别

- 都是在客户端以键值对存储的存储机制，并且只能将值存储为字符串。

|                                                    | cookie                                             | localStorage | sessionStorage |
| -------------------------------------------------- | -------------------------------------------------- | ------------ | -------------- |
| 由谁初始化                                         | 客户端或服务器，服务器可以使用`Set-Cookie`请求头。 | 客户端       | 客户端         |
| 过期时间                                           | 手动设置                                           | 永不过期     | 当前页面关闭时 |
| 在当前浏览器会话（browser sessions）中是否保持不变 | 取决于是否设置了过期时间                           | 是           | 否             |
| 是否随着每个 HTTP 请求发送给服务器                 | 是，Cookies 会通过`Cookie`请求头，自动发送给服务器 | 否           | 否             |
| 容量（每个域名）                                   | 4kb                                                | 5MB          | 5MB            |
| 访问权限                                           | 任意窗口                                           | 任意窗口     | 当前页面窗口   |

#### 什么是 WebSQL？

- WebSQL 是使用 SQL 的客户端（浏览器）的数据库 API。
- Web SQL 数据库 API 并不是 HTML5 规范的一部分，但是它是一个独立的规范，引入了一组使用 SQL 操作客户端数据库的 APIs。
- 并非所有浏览器都支持[WebSQL](https://caniuse.com/#search=websql)。
- 现在[不推荐使用](https://www.w3.org/TR/webdatabase/) WebSQL ，而是使用 IndexedDB 代替它。

#### 什么是 IndexedDB？

> IndexedDB 是一种底层异步 API，浏览器内置的数据库，用于在客户端存储大量的结构化数据（也包括文件/二进制大型对象（blobs））。

- 它将将数据存储为键值对。
- 大多数浏览器都支持 [IndexedDB](http://caniuse.com/#feat=indexeddb)。

> IndexedDB API 功能强大，但对于简单的情况可能看起来太复杂如果你更喜欢一个简单的 API，请尝试 [localForage](https://localforage.github.io/localForage/)，[dexie.js](http://www.dexie.org/)，[PouchDB](https://pouchdb.com/)，[IDB](https://www.npmjs.com/package/idb)，[IDB-KEYVAL](https://www.npmjs.com/package/idb-keyval)，[JsStore](https://jsstore.net/) 或者 [lovefield](https://github.com/google/lovefield) 之类的库，这些库使 IndexedDB 对开发者来说更加友好。

- [IndexedDB](https://developer.mozilla.org/zh-CN/docs/Web/API/IndexedDB_API)
- [浏览器数据库 IndexedDB 入门教程](http://www.ruanyifeng.com/blog/2018/07/indexeddb.html)

#### 为什么在 HTML5 中使用 IndexedDB 代替 WebSQL？

IndexedDB 像一个 NoSQL 数据库，而 WebSQL 像关系型数据库，使用 SQL 查询数据。W3C WebSQL 已经不再支持这种技术。

#### HTML5 应用程序缓存（Application Cache）

根据最新的标准，该特性已经从 Web 标准中删除，建议使用 [Service Workers](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API/Using_Service_Workers) 代替。这里找了一些资料，感兴趣了解一下。

- [有趣的 HTML5：离线存储](http://segmentfault.com/a/1190000000732617)
- [Using the application cache](https://developer.mozilla.org/en-us/docs/Web/HTML/Using_the_application_cache)
- [HTML5-离线缓存（Application Cache）](https://mp.weixin.qq.com/s/Q-Z8kYWSUJpkpAkTBv1Igw)

### 什么是 Web Workers？

- Web Workers 帮助我们在后台运行 JavaScript 代码，而不会阻止应用程序。
- Web Workers 在一个隔离的（新的）线程中运行，用于执行 JavaScript 代码，并且通过 postMessage 将结果回传到主线程。这样就不会阻塞主线程的运行。
- Web Workers 通常用于大型任务。
- Web Workers 需要一个单独的文件来存储我们的 JavaScript 代码。
- Web Workers 文件是异步下载的 。
- 所有最新的浏览器均支持 [Web Worker](https://caniuse.com/webworkers)。

客户端 js：

```js
var myWebWorker = new Worker('task.js') // 创建 worker

// 监听 task.js worker 消息
worker.addEventListener(
  'message',
  function (event) {
    console.log('Worker said: ', event.data)
  },
  false
)

// 启动工作程序
worker.postMessage('From web worker file')
```

task.js（工作文件）文件：

```js
// 监听客户端 JS 文件发布消息
self.addEventListener(
  'message',
  function (event) {
    // 处理后的数据发送到客户端监听 JS 文件
    self.postMessage(event.data)
  },
  false
)
```

- [使用 Web Worker 加速 JavaScript 应用程序](https://blog.teamtreehouse.com/using-web-workers-to-speed-up-your-javascript-applications)

### WebSocket

- WebSocket 是 HTML5 开始提供的一种在单个 TCP 连接上进行全双工通讯的协议。
- WebSocket 是一种在客户端与[服务器](https://developer.mozilla.org/zh-CN/docs/Glossary/Server)之间保持[TCP](https://developer.mozilla.org/zh-CN/docs/Glossary/TCP)长连接的[网络协议](https://developer.mozilla.org/zh-CN/docs/Glossary/Protocol)，可以随时进行信息交换。
- WebSocket 使用 ws 或 wss 的统一资源标志符，类似于 HTTPS，其中 wss 表示在 TLS 之上的 Websocket。
- 默认情况下，Websocket 协议使用 80 端口；运行在 TLS 之上时，默认使用 443 端口。

**WebSocket 如何兼容低浏览器？**

- Adobe Flash Socket
- ActiveX HTMLFile（IE）
- 基于 multipart 编码发送 XHR
- 基于长轮询的 XHR

**websocket 与 socket 的区别**：

Socket 是传输控制层协议，WebSocket 是应用层协议。更多请看参考

**更多学习资料**：

- [WebSocket 介绍和 Socket 的区别](https://blog.csdn.net/qushaming/article/details/90747326)
- [WebSocket 教程](http://www.ruanyifeng.com/blog/2017/05/websocket.html)
- [WebSocket](https://developer.mozilla.org/zh-CN/docs/Web/API/WebSocket)
- [WebSocket 是什么原理？为什么可以实现持久连接？](https://www.zhihu.com/question/20215561/answer/40316953)

### Geolocation API 如何在 HTML5 中工作？

- HTML5 [Geolocation API](https://developer.mozilla.org/zh-CN/docs/Web/API/Navigator/geolocation)（地理定位）允许用户在需要时向 web 应用程序提供用户的位置。出于隐私原因，用户需要获得报告位置信息的权限。
- JavaScript 可以捕获你的纬度和经度，并可以发送到后端 Web 服务器，做一些奇特的位置感知的事情，比如找到本地企业或在地图上显示你的位置
- 如今，大多数浏览器和移动设备都[支持](https://www.caniuse.com/?search=geolocation) Geolocation API
- Geolocation API 通过 `navigator` 获取对象。

```js
if ('geolocation' in navigator) {
  /* geolocation 是可用的 */
} else {
  /* geolocation 是不可用的 */
}
```

- 使用 `navigator.geolocation.getCurrentPosition()` 方法获取用户的位置

### 页面可见性（Page Visibility）API 可以有哪些用途？

- 页面可见性 API 提供了您可以观察的事件，刹车了解文档何时可见或隐藏，以及查看页面当前可见性状态的功能。
- 使用选项卡式浏览，任何给定网页都有可能在后台，因此对用户不可见。页面可见性 API 提供了您可以观察的事件，以便了解文档何时可见或隐藏，以及查看页面当前可见性状态的功能。
- `document.hidden` 返回一个布尔值。
  - true 表示页面可见，false 则表示页面隐藏。
  - 不同页面之间来回切换，将触发 [visibilitychange](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/visibilitychange_event) 事件。
- `document.visibilityState`：表示页面所处的状态，当前页面的可见性，有四个取值
  - `visible`：页面彻底不可见。
  - `hidden`：页面至少一部分可见。
  - `prerender`：页面即将或正在渲染，处于不可见状态。
  - `unloaded`：已被废弃，不在使用。
- 只要 `document.visibilityState` 属性发生变化，就会触发 `visibilitychange` 事件

```js
// 打开新的页面，来回切换标签页，观察页面标题的变化
document.addEventListener('visibilitychange', function () {
  if (document.visibilityState === 'hidden') {
    document.title = '爱我'
  } else {
    document.title = '恨我'
  }
})
```

**用途**：

- 动画，视频，音频都可以在页面显示时打开，在页面隐藏时关闭
- 完成登陆后，无刷新自动同步其他页面的登录状态

```js
// 视频暂停或播放
document.addEventListener('visibilitychange', function () {
  if (document.visibilityState === 'hidden') {
    video.pause()
  } else if (document.visibilityState === 'visible') {
    video.play()
  }
})
```

- tips：页面可见性 API 对于节省资源和提高性能特别有用，它使页面在文档不可见时避免执行不必要的任务。
- [Page Visibility（页面可见性）](https://developer.mozilla.org/zh-CN/docs/Web/API/Page_Visibility_API)
- [Page Visibility API 教程](http://www.ruanyifeng.com/blog/2018/10/page_visibility_api.html)

### 说一下 HTML5 Drag And Drop API

> **HTML 拖放（Drag and Drop）**接口使应用程序能够在浏览器中使用拖放功能。例如，用户可使用鼠标选择可拖拽（draggable）元素，将元素拖拽到可放置（droppable）元素，并释放鼠标按钮以放置这些元素。

| Event     | Description                                                                                    |
| --------- | ---------------------------------------------------------------------------------------------- |
| Drag      | 每次拖动对象时移动鼠标时，它都会激发。事件主体是被拖放元素，在正在拖放被拖放元素时触发。       |
| Dragstart | 当用户开始拖动对象时触发。事件主体是被拖放元素，在开始拖放被拖放元素时触发。                   |
| Dragenter | 当用户将鼠标光标移到目标元素上时，它将激发。事件主体是目标元素，在被拖放元素进入某元素时触发。 |
| Dragover  | 当鼠标移到某个元素上时触发此事件。事件主体是目标元素，在被拖放在某元素内移动时触发。           |
| Dragleave | 当鼠标离开元素时触发此事件。事件主体是目标元素，在被拖放元素移出目标元素是触发。               |
| Drop      | 拖放操作结束时触发。事件主体是目标元素，在目标元素完全接受被拖放元素时触发。                   |
| Dragend   | 当用户释放鼠标按钮以完成拖动操作时触发。事件主体是被拖放元素，在整个拖放操作结束时触发         |

[draggable](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/draggable) 是可枚举的属性指示该元素是否可以拖动，用于标识元素是否允许使用 [HTML 拖放 API](https://developer.mozilla.org/en-US/docs/Web/API/HTML_Drag_and_Drop_API)

```html
<div id="div1" ondrop="drop(event)" ondragover="allowDrop(event)"></div>
<img
  id="drag1"
  src="img_logo.gif"
  draggable="true"
  ondragstart="drag(event)"
  width="336"
  height="69"
/>
<script>
  function allowDrop(e) {
    e.preventDefault()
  }

  function drag(e) {
    e.dataTransfer.setData('text', e.target.id)
  }

  function drop(ev) {
    e.preventDefault()
    var data = e.dataTransfer.getData('text')
    e.target.appendChild(document.getElementById(data))
  }
</script>
```

- [HTML Drag and Drop API](https://developer.mozilla.org/zh-CN/docs/Web/API/HTML_Drag_and_Drop_API)
- [确定从 Dragenter 和 Dragover 事件中拖动的内容](https://stackoverflow.com/questions/11065803/determine-what-is-being-dragged-from-dragenter-dragover-events)
- [触摸屏设备上的 HTML5 Drag and Drop API](https://stackoverflow.com/questions/3382393/html5-drag-and-drop-api-on-touch-screen-devices)
- [HTML5 在窗口间 drag and drop](https://stackoverflow.com/questions/3694631/html5-drag-and-drop-between-windows)

### 其他 HTML5 API

#### document.querySelector() 和 document.querySelectorAll()

- `querySelector()`：根据 css 选择器返回第一个匹配的元素，如果没有匹配返回 null
- `querySelectorAll()`：方法返回文档中匹配指定 CSS 选择器的所有元素，返回 NodeList 对象。如果 querySelectorAll 没有匹配的内容返回的是一个空数组。

#### classList

- 控制 CSS 的 增、删、切换、是否存在某个类

```js
ele.classList.add('addClass')
ele.classList.remove('removeClass')
ele.classList.toggle('toggleClass')
ele.classList.contains('containsClass')
```

#### contextMenu

- 它并不会替换原有的右键菜单，而是将你的自定义右键菜单添加到浏览器的右键菜单里

```html
<div id="menu">Lorem ipsum dolor sit amet.</div>
<script>
  menu.addEventListener('contextmenu', function () {
    alert('点我！')
  })
</script>
```

你也可以阻止它，显示自己自定义的菜单

```js
menu.addEventListener('contextmenu', function (e) {
  e.preventDefault()
  // ...
})
```

- [如何为您的 Web 应用程序创建自定义上下文菜单](https://dev.to/iamafro/how-to-create-a-custom-context-menu--5d7p)

#### dataset

- 通过 [`dataset`](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLElement/dataset) 可以方便的获取或设置 `data-*` 自定义数据属性集

```html
<div
  class="avatar"
  data-user="lisi"
  data-avatar-type="circle"
  data-animateSpeed
>
  lorem
</div>
<script>
  const avatar = document.querySelector('.avatar')
  avatar.dataset.user === 'lisi' // true
  avatar.dataset.avatarType === 'circle' // true
  avatar.dataset.animateSpeed = 4000

  // 添加不存在的属性
  avatar.dataset.id = 'user'
  // console.log(avatar.dataset)
</script>
```

#### tabindex

- `tabindex` 属性规定当使用 "tab" 键进行导航时元素的顺序。
- 在 HTML4.01 中，tabindex 属性可用于：`<a>`，`<area>`，`<button>`，`<input>`，`<object>`，`<select>` 和 `<textarea>`。
  - 在 HTML5 中，`tabindex` 属性可用于任何的 HTML 元素（它会验证任何 HTML 元素。但不一定是有用）

```html
<ul>
  <li tabindex="2">HTML</li>
  <li tabindex="1">CSS</li>
  <li tabindex="3">JAVASCRIPT</li>
</ul>
```

#### accessKey

- `accessKey` 属性规定激活（使元素获得焦点）元素的快捷键。
- 不同浏览器使用的快捷键方法不同：
  - IE，Chrome，Safari，Opera 15+：[ALT] + accesskey
  - Opera prior version 15：[SHIFT] [ESC] + accesskey
  - Firefox：[ALT] [SHIFT] + accesskey

```html
<input accesskey="b" />
<a href="https://www.baidu.com/" accesskey="c">百度一下，你就知道</a>
```

#### FullScreen（全屏）

- [FullScreen](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/fullScreen) API 是一个新的 JavaScript API
- 全屏显示可以是任意元素
- HTML5 API 存在兼容性问题（IE9+），即使高版本浏览器也有兼容性问题
- 不同浏览器需要添加不同的前缀 webkit、moz、o、ms

```js
const fullscreen = (mode = true, el = 'body') =>
  mode
    ? document.querySelector(el).requestFullscreen()
    : document.exitFullscreen()

fullscreen() // 默认全屏模式打开 "body"
fullscreen(false) // 退出全屏模式
```

`:fullscreen` CSS 伪元素代表一个元素，当浏览器是在全屏模式下的显示。

```css
.elem:fullscreen {
  background-color: #e4708a;
  width: 100vw;
  height: 100vh;
}
```

### 预加载

- 预取 CSS 文件，预渲染整个页面或提前解析域
- 浏览器有一个简单的内置方式来完成所有这些事情。有六个 `<link rel>` 标签指示浏览器预加载内容：

```html
<link rel="prefetch" href="/index.css" as="style" />
<link rel="preload" href="/index.css" as="style" />
<link rel="preconnect" href="https://example.com" />
<link rel="dns-prefetch" href="https://example.com" />
<link rel="prerender" href="https://example.com/about.html" />
<link rel="modulepreload" href="/index.js" />
```

#### preload

- 使用 `preload` 作为 `rel` 属性的属性值。还需要通过 `href` 和 `as` 属性指定需要被预加载资源的资源路径及其类型。

```html
<link rel="preload" href="index.css" as="style" />
```

- 使用 `as` 来指定将要预加载的内容的类型，将使得浏览器能够：
  - 更精确地优化资源加载优先级。
  - 匹配未来的加载需求，在适当的情况下，重复利用同一资源。
  - 为资源应用正确的[内容安全策略](https://developer.mozilla.org/en-US/docs/Web/HTTP/CSP)。
  - 为资源设置正确的 [`Accept`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Accept) 请求头。
  - [MDN 完整列表](https://developer.mozilla.org/en-US/docs/Web/HTML/Preloading_content#what_types_of_content_can_be_preloaded)

#### prefetch

- `<link rel="prefetch">` 要求浏览器在后台下载并缓存资源（例如 JS 或 CSS）。下载的优先级较低，因此不会干扰更重要的资源。当您知道在下一个页面上需要该资源，并且想要提前对其进行缓存时，这将很有帮助。
- 下载资源后，浏览器不执行任何操作。不执行 JS，不应用 CSS。它只是被缓存了，因此当其他需求时，它立即可用。
- 通过 `link` 标签的 `rel` 属性指定为 `"prefetch"`，在 `href` 属性里指定要加载资源的地址

```html
<!-- 预加载整个页面 -->
<link rel="prefetch" href="https://juejin.cn/user/96412754251390" />

<!-- 预加载一个图片 -->
<link
  rel="prefetch"
  href="https://images.pexels.com/photos/918281/pexels-photo-918281.jpeg?auto=compress&cs=tinysrgb&dpr=1&w=500"
/>
```

#### preconnent

- `<link rel="preconnect">` 要求浏览器提前执行到域的连接。

```html
<link rel="dns-prefetch" href="https://juejin.cn/user/96412754251390" />
```

#### dns-prefetch

- `DNS-prefetch`（DNS 预获取）是尝试在请求资源之前解析域名。这可能是后面要加载的文件，也可能是用户尝试打开的链接目标。

```html
<link rel="dns-prefetch" href="https://juejin.cn/user/96412754251390" />
```

**链接预加载的一些注意事项**

- 预加载可以跨域进行，当然，请求时 cookie 等信息也会被发送。
- 预加载可能破坏网站统计数据，而用户并没有实际访问。
- 浏览器兼容性不是很好

#### prerender

- `<link rel="prerender">` 要求浏览器加载 URL 并将其呈现在不可见的标签中。当用户单击指向该 URL 的链接时，应立即呈现该页面。当您确实确定用户接下来将访问特定页面并且想要更快地呈现它时，这将很有帮助。

```html
<link rel="prerender" href="https://juejin.cn/user/96412754251390" />
```

- 当您确定大多数用户将导航到特定页面时，您希望加快速度，那么你可以使用它

#### modulepreload

- `<link rel="modulepreload">` 告诉浏览器尽快下载，缓存和编译 JS 模块脚本。
- 使用它可以更快地加载您的 ES 模块应用程序。此标签仅适用于预加载 ES 模块——即您通过 `import ...` 或导入的模块 `<script type="module">`。

```html
<link rel="modulepreload" href="/static/Header.js" />
```

- [Preload, prefetch and other `<link>` tags](https://3perf.com/blog/link-rels/#preload)
- [dns-prefetch](https://developer.mozilla.org/zh-CN/docs/Web/Performance/dns-prefetch)
- [通过 rel="preload"进行内容预加载](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Preloading_content)
- [预取，预加载，预浏览](https://css-tricks.com/prefetching-preloading-prebrowsing/)

## 其他

### 对于 WEB 标准以及 W3C 的理解与认识问题

- **web 标准**简单来说可以分为**结构、表现和行为**。
  - 结构主要是有 HTML 标签组成。或许通俗点说，在页面 body 里面我们写入的标签都是为了页面的结构。
  - 表现即指 css 样式表，通过 css 可以是页面的结构标签更具美感。
  - 行为是指页面和用户具有一定的交互，同时页面结构或者表现发生变化，主要是由 js 组成。
- web 标准一般是将该三部分独立分开，使其更具有模块化。但一般产生行为时，就会有结构或者表现的变化，也使这三者的界限并不那么清晰。
- 万维网联盟（[W3C](https://www.w3.org/)）是一个国际组织，它开发开放标准以确保 Web 的长期发展。
- W3C 对 web 标准提出了规范化的要求，也就是在实际编程中的一些代码规范：
  - web 标准规范要求，书写标签**必须闭合、标签小写、不乱嵌套**，标签规范可以提高搜索引擎对页面的抓取效率，对 SEO 很有帮助
  - 建议使用外链 CSS 和 JS 脚本，从而达到结构、表现与行为的分离，提高页面的渲染速度，提高用户的体验
  - 样式与标签的分离，更合理的语义化标签，使内容能被更多的用户所访问、内容能被更广泛的设备所访问、更少的代码和组件， 从而降低维护成本、改版方便
  - 不需要变动页面内容，便可提供打印版本而不需要复制内容，提高网站易用性；
  - 遵循 w3c 制定的 web 标准，能够使用户浏览者更方便的阅读，使网页开发者之间更好的交流。

### 前端页面有哪三层构成，分别是什么？作用是什么？

- 分成：结构层、表示层、行为层。
- 结构层（structural layer）：由 HTML 或 XHTML 之类的标记语言负责创建。标签，也就是那些出现在尖括号里的单词，对网页内容的语义含义做出了描述，但这些标签不包含任何关于如何显示有关内容的信息。例如，P 标签表达了这样一种语义：“这是一个文本段。”
- 表示层（presentation layer）：由 CSS 负责创建。 CSS 对“如何显示有关内容”的问题做出了回答。
- 行为层（behaviorlayer）：负责回答“内容应该如何对事件做出反应”这一问题。这是 JavaScript 语言和 DOM 主宰的领域。

### 什么是渐进式渲染？

- 渐进式渲染（Progressive rendering）：是用于提高网页性能（尤其是提高用户感知的加载速度），以尽快呈现页面的技术。
- 此类技术的示例：
  - 图片懒加载：页面上的图片不会一次性全部加载。当用户滚动页面到图片部分时，JavaScript 将加载并显示图像。
  - 确定显示内容的优先级（Hierarchical rendering）：为了尽快将页面呈现给用户，页面只包含基本的最少量的 CSS、脚本和内容，然后可以使用延迟加载脚本或监听 `DOMContentLoaded`/`load` 事件加载其他资源和内容。
  - 异步加载 HTML 片段：当页面通过后台渲染时，把 HTML 拆分，通过异步请求，分块发送给浏览器。
- [异步片段：使用 Marko 重新发现渐进式 HTML 渲染](https://tech.ebayinc.com/engineering/async-fragments-rediscovering-progressive-html-rendering-with-marko/)
- [什么是渐进式渲染？](https://stackoverflow.com/questions/33651166/what-is-progressive-rendering)

### 你能描述一下渐进增强和优雅降级之间的不同吗？

[渐进增强（Progressive enhancement）](https://developer.mozilla.org/en-US/docs/Glossary/Progressive_Enhancement)

[优雅降级（Graceful degradation）](https://developer.mozilla.org/en-US/docs/Glossary/Graceful_degradation)

### 什么是微格式？在前端构建中应该考虑微格式吗？

- 微格式（Microformats）是一种让机器可读的语义化 XHTML 词汇的集合，是结构化数据的开放标准。是为特殊应用而制定的特殊格式。
- 优点：将智能数据添加到网页上，让网站内容在搜索引擎结果界面可以显示额外的提示。（如：豆瓣，有兴趣自行 google）
- [Microformats](https://developer.mozilla.org/en-US/docs/Web/HTML/microformats)

### 什么是字符编码？

- 字符编码是一种将字节转换为字符的方法。为了正确地验证或显示 HTML 文档，程序必须选择适当的字符编码。这是在标签中指定的：

```html
<meta charset="UTF-8" />
```

- **UTF-8**：Unicode 转换格式，以 8 位为单位，即以字节为单位。UTF8 中的字符长度可以从 1 到 4 个字节，从而使 UTF8 的宽度可变。

### 什么是 WHATWG？

[WHATWG](https://whatwg.org/)（Web 超文本应用技术工作组）是一个对通过标准和测试来发展 Web 感兴趣的人们组成的社区。

### 什么是 WebP？

- [WebP](https://developers.google.com/speed/webp/) 类似于 JPG、PNG 这样的图像格式，它的大小比其他格式小大约 10-20%。
- 由 Google 在 2010 年开发和推出。
- 并非所有浏览器都支持 WebP。
- 可以使用插件将其他格式转换为 WebP。

### 什么是 Web 组件？

> [Web Components](https://developer.mozilla.org/en-US/docs/Web/Web_Components) 是一套不同的技术，允许您创建可重用的定制元素（它们的功能封装在您的代码之外）并且在您的 web 应用中使用它们。不需要需要任何外部库来工作。

**特征**：

- **Custom elements（自定义元素）**
- **Shadow DOM（影子 DOM）**
- **HTML templates（HTML 模板）**
- **HTML Import** 允许导入的外部 HTML 文档。

**更多资料**

- [Web 组件入门实例教程](http://www.ruanyifeng.com/blog/2019/08/web_components.html)

### Web 应用程序中的可访问性？

> 维基百科：可访问性是最常用于描述设施或设施，帮助残疾人，如“轮椅”。这可以扩展到盲文标识、轮椅坡道，音频信号在人行横道，轮廓人行道，网站设计，等等。

### 什么是 ARIA？

> **[Accessible Rich Internet Applications（ARIA）**](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Roles/Region_role) 是能够让残障人士更加便利的访问 Web 内容和使用 Web 应用（特别是那些由 JavaScript 开发的）的一套机制。

- ARIA 通过 HTML 属性为屏幕阅读器提供了额外的信息。其不影响元素如何被呈现在浏览器中。
- 您可以通过遵循 ARIA 标准（例如：HTML 语义，alt 属性并以预期的方式使用 `[role = button]`）来使您的网站更易于访问

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

- ARIA `role` 没有为大多数元素的默认语义添加任何内容
- 在某些情况下，HTML 元素的语义可以通过 ARIA `role`，状态或属性来表达。常被称为元素的“[默认隐式 ARIA 语义](https://www.w3.org/TR/wai-aria-1.1/#implicit_semantics)”
- ARIA 允许开发人员以有意义的方式重新发明和扩展原生 HTML 特性。但它的特性与内置技术相比是脆弱的。

一些冗余 ARIA 的示例：

```html
<button role="button">关注</button>
<a href="https://github.com/lio-zero" role="link">lio-zero</a>
<input type="checkbox" role="checkbox" />
<input type="radio" role="radio" />
```

HTML5 使用默认的隐式语义定义了一组新的结构和分段元素，这些语义与 ARIA `role` 匹配（在某些情况下）：

- 使用 `role = button` 时，考虑使用 `<button>` 元素，或者其他各种原生 HTML 按钮类型。
- 使用 `role=link` 时，考虑改用 `<a href>` 元素。
- 使用 `role=heading` 和 `aria-level="1-6"`，考虑改用 `<h1>` 到 `<h6>` 元素。
- 使用 `role=list` 和 `role=listitem` 时，考虑改用 `<ol>` 或 `<ul>` 和 `<li>` 元素。
- 使用 `role=listbox` 和 `role=option`，考虑改用 `<select>` 和 `<option>` 元素。
- 使用 `role=checkbox` 或 `role=radio` 时，考虑改用 `<input type="checkbox">` 或 `<input type="radio">` 元素。
- 使用 `role=textbox`，可以考虑使用 `<input type="text">` 或搜索、电子邮件、url 或电话。
- `article`、`aside`、`footer`、`header`、`main`、`nav`、`section` 等等。

这意味着在实现后，浏览器将公开该元素的默认隐式语义，因此您不必这样做。

更多资料：

- [在 HTML 和 ARIA 大括号上（默认的隐式 ARIA 语义，他们不想让你知道）](http://html5doctor.com/on-html-belts-and-aria-braces/)
- [HTML5 – W3C 建议书 2014 年 10 月 28 日](http://www.w3.org/TR/html5/)
- [在 HTML 中使用 ARIA 的注意事项](http://w3c.github.io/aria-in-html/)

### 什么是屏幕阅读器？

屏幕阅读器是提供辅助技术的软件程序，该技术可以使残障人士（例如，没有视力，声音或滑鼠能力的人）使用 Web 应用程序。

## HTML 开发准则

- [符合 W3C](https://validator.w3.org/?ref=frontendchecklist)：所有页面都需要使用 W3C 验证程序进行测试，以识别 HTML 代码中可能存在的问题。
- 清理注释：在将页面发送到生产环境之前，需要删除不必要的代码。
- 错误页面：每个网站都应该存在错误 404 页面和 5xx。
- [HTML5 语义元素](https://htmlreference.io/semantic/)：适当使用了 HTML5 语义元素（`<header>`，`<section>`，`<footer>`，`<main>`...）
- [HTMLHint](https://htmlhint.com/)：我使用工具来帮助我分析我的 HTML 代码可能遇到的任何问题。
  - [Dirty markup](https://dirtymarkup.com/?ref=frontendchecklist)
  - [webhint](https://webhint.io/?ref=frontendchecklist) 是一个可自定义的整理工具，可通过检查代码中的最佳做法和常见错误来帮助您提高网站的可访问性，速度，跨浏览器兼容性以及其他功能。
- [链接检查器](https://validator.w3.org/checklink?ref=frontendchecklist)：检查页面链接是否可用，请确认您没有任何 404 错误。
- [Noopener](https://mathiasbynens.github.io/rel-noopener/?ref=frontendchecklist)：如果您正在使用带有 `target ="_ blank"` 的外部链接，则您的链接应具有 `rel="noopener"` 属性，以防止标签被挪用。如果您需要支持旧版本的 Firefox，请使用 `rel="noopener noreferrer"`
- [HTML 代码规范](https://codeguide.co/#html)：开发一致、灵活和可持续的 HTML 和 CSS 的标准。
- 使用出色的开源工具 [W3C tools](https://w3c.github.io/developers/tools/) 将代码发挥最大潜能。

## HTML 性能优化

- [为页面测速](https://varvy.com/pagespeed/style-script-order.html)制定样式和脚本
- 压缩 HTML：将注释、空格和空行从生产文件中删除。
  - 删除所有不必要的空格、注释和中断行将减少 HTML 的大小，加快网站的页面加载时间，并显著减少用户的下载时间。
  - 可以使用 Glup 等构建工具进行删除
  - [HTML minifier | Minify Code](http://minifycode.com/html-minifier/)
  - [Experimenting with HTML minifier — Perfection Kills](http://perfectionkills.com/experimenting-with-html-minifier/#use_short_doctype)
- 删除不必要的属性：像 `type="text/javascript"` or `type="text/css"` 这样的属性应该被移除。
  - 类型属性不是必需的，因为 HTML5 把 `text/css` 和 `text/javascript` 作为默认值。没用的代码应在网站或应用程序中删除，因为它们会使网页体积增大。
  - 确保所有和 `<script>` 标签都没有任何 `type` 属性。
  - [The Script Tag | CSS-Tricks](https://css-tricks.com/the-script-tag/)
- 避免脚本阻塞加载。确保在使用 JavaScript 代码之前加载 CSS。
  - 在引用 JavaScript 之前引用 CSS 可以实现更好地并行下载，从而加快浏览器的渲染速度。
  - 确保 `<link>` 和 `<style>` 始终位于 `<script>` 之前。
  - [合理安排 styles 和 scripts 来提高网页速度](https://varvy.com/pagespeed/style-script-order.html)
- 尽可能使用 `async` 和 `defer`
  - 确保 JavaScript 脚本兼容 `async` 和 `defer`，任何时候都要尽可能使用 `async`，特别是当你有较多的 `script` 标签时。
  - 这样在加载 JavaScript 的过程中页面就不会重新绘制，否则，浏览器在不具有这些特性的 `script` 标签后就不会重绘任何东西。
  - [消除渲染阻塞资源](https://web.dev/render-blocking-resources/)
- DNS 预取：一次 DNS 查询时间大概在 60-120ms 之间或者更长，提前解析网页中可能的网络连接域名

```html
<link rel="dns-prefetch" href="http://example.com/" />
```

- 减少内联脚本的数量
  - 内联脚本在页面加载过程中消耗很多资源，因为解析器认为内联脚本会改变页面结构。
  - 通常，尽量少的使用内联脚本和减少用 `document.write()` 来输出内容，在一定情况下可以加速整体页面的载入。现在浏览器中一般使用现代的 W3C DOM 方法操作页面内容，优于使用 `document.write()` 的传统方法。
- 缩小和压缩图像
  - 较大的图像会导致页面需要更多的时间来加载。在将图像添加到页面之前，请考虑使用 Photoshop 等图像处理工具内置的压缩功能，或使用 [Compress JPEG](https://compressjpeg.com/) 或 [Tiny PNG](https://tinypng.com/) 等专用工具对图像进行压缩
- 最小化文件数量
  - 减少一个页面引用的文件数量可以降低在下载一个页面的过程中需要的 [HTTP](https://developer.mozilla.org/en-US/docs/HTTP) 请求数量，从而减少这些请求的收发时间。
  - 根据其缓存设置，浏览器可能会为每个所引用的文件发送一个带 [If-Modified-Since](https://wiki.developer.mozilla.org/en-US/docs/Web/HTTP/Headers/If-Modified-Since) 的请求给网络服务器，以查询这些文件自上次加载后是否有被修改。查询引用文件上次修改时间会花费太多时间，导致网页首屏延迟，这是因为在渲染页面之前浏览器必须确认每个文件的修改时间。
- 最小化 `iframe` 的数量：仅在没有任何其他技术可行性时才使用 `iframe`。
- 避免节点深层级嵌套：深层级嵌套的节点在初始化构建时往往需要更多的内存占用，并且在遍历节点时也会更慢些，这与浏览器构建 DOM 文档的机制有关。浏览器会把整个 HTML 文档的结构存储为 DOM "树" 结构。当文档节点的嵌套层次越深，构建的 DOM 树层次也会越深。
- 页面缓存：在不设置页面缓存的情况下，每次刷新页面会重新读取服务器文件。设置页面缓存，每次刷新可从本地读取，提高页面加载效率
  - 通过设置页面头的 `expires` 来定义页面过期时间，将过期时间定久一点就达到了 "永久" 缓存。

```html
<meta http-equiv="expires" content="Sunday 26 October 2099 01:00 GMT" />
```

- 避免 Table 布局：`table` 比其它 HTML 标签占更多的字节（造成下载时间延迟，占用服务器更多流量资源）
  - 不使用 `table` 布局，而应运用 `float`，`position`，`flex` 或 `grid` 来布局。
  - 当然，`table` 仍是不失为一种有效的展示表格数据的方式。为了帮助浏览器更快速的渲染你的页面，你应该避免嵌套 `table`。
  - 参考：[为什么我们不建议用 Table 布局](https://www.html5tricks.com/why-not-table-layout.html) 和 [最小化布局](https://medium.com/better-programming/web-performance-dom-reflow-76ac7c4d2d4f)
- [如何制作快速加载的 HTML 页面](https://developer.mozilla.org/zh-CN/docs/Web/Guide/HTML/Tips_for_authoring_fast-loading_HTML_pages) 中还有其他方面的例子，如：高效地排列页面组件、合理的选择 user-agent 等
- [优先分配资源](https://web.dev/prioritize-resources/)
- [预加载关键资源以提高加载速度](https://web.dev/preload-critical-assets/)
- [尽早建立网络连接以提高感知的页面速度](https://web.dev/preconnect-and-dns-prefetch/)
- [预取资源以加速将来的导航](https://web.dev/link-prefetch/)
- [Best Practices for Speeding Up Your Web Site](http://developer.yahoo.com/performance/rules.html)
- [基于 JavaScript 和网络信息 API 的自适应服务](https://addyosmani.com/blog/adaptive-serving/)

## 参考资料

- [HTML 最佳实践](https://github.com/hail2u/html-best-practices)
- [W3C](https://www.w3.org/)
- [维基百科](https://zh.wikipedia.org/wiki/Wikipedia:%E9%A6%96%E9%A1%B5)
- [MDN](https://developer.mozilla.org/en-US/docs/Learn/CSS/CSS_layout/Introduction)
- [HTML 标准](https://html.spec.whatwg.org/multipage/)
- [front-end-interview-handbook](https://github.com/yangshun/front-end-interview-handbook)
- [front-end-Interview-Questions](https://github.com/khan4019/front-end-Interview-Questions)
- [Front-End-Performance-Checklist](https://github.com/thedaviddias/Front-End-Performance-Checklist)
- [Front-End-Checklist](https://github.com/thedaviddias/Front-End-Checklist)
- [30-seconds-of-interviews](https://github.com/30-seconds/30-seconds-of-interviews)
- [HTML 备忘单](https://htmlcheatsheet.com/)
