# 鲜为人知的 HTML 属性

## contenteditable

`contenteditable` 表示元素内容是否可被用户编辑。

它可以有以下值：

- `true` 或者空字符串，表示元素是可被编辑的。
- `false` 表示元素不可被编辑。
- `inherit` 表示元素继承其父元素的可编辑状态。

```html
<div contenteditable="true">
  <h1>元素可编辑</h1>
</div>
<div contenteditable="">
  <h1>元素可编辑</h1>
</div>
<div contenteditable="false">
  <h1>元素不可编辑</h1>
</div>
<div contenteditable="inherit">
  <h1>元素继承其父元素的可编辑状态</h1>
</div>
```
> 推荐：[为 contenteditable 元素添加占位符](https://github.com/lio-zero/blog/blob/main/JavaScript/%E4%B8%BA%20contenteditable%20%E5%85%83%E7%B4%A0%E6%B7%BB%E5%8A%A0%E5%8D%A0%E4%BD%8D%E7%AC%A6.md)。

## dir

`dir` 属性规定元素内容的文本方向。

它的取值如下：

- `ltr`（默认）— 从左向右的文本方向。常用于那种从左向右书写的语言（比如英语）。
- `rtl` — 从右向左的文本方向。常用于那种从右向左书写的语言（比如阿拉伯语）。
- `auto` — 让浏览器根据内容来判断文本方向。它在解析元素中字符时会运用一个基本算法，直到发现一个具有强方向性的字符，然后将这一方向应用于整个元素。仅在文本方向未知时推荐使用。

```html
<p dir="rtl">
  This paragraph is in English but incorrectly goes right to left.
</p>
<p dir="ltr">This paragraph is in English and correctly goes left to right.</p>
<p>هذه الفقرة باللغة العربية ولكن بشكل خاطئ من اليسار إلى اليمين.</p>
<p dir="auto">
  هذه الفقرة باللغة العربية ، لذا يجب الانتقال من اليمين إلى اليسار.
</p>
```

## hidden

`hidden` 用于指定元素是否隐藏，可用于动态隐藏/显示元素而不用修改元素的 CSS 样式。

```html
<div hidden>lorem</div>
<div hidden="true">lorem</div>
<div hidden="false">lorem</div>
```

你可以配合 JS 来改变其值为 `true` 或 `false`，让浏览器知道是否需要渲染此类元素。

## title

HTML `title` 包含表示与其所属元素相关的建议信息的文本。也就是指定元素的提示文本。

```html
<p title="爱在这">爱在这</p>
```

当鼠标移动到带有 `title` 属性的元素上时，提示文本将作为工具提示（tooltip）显示出来。可以说，`title` 是对该元素的描述和进一步的说明。

推荐阅读 [HTML title 属性](https://github.com/lio-zero/blog/blob/master/HTML/HTML%20title%20%E5%B1%9E%E6%80%A7.md)和 [alt 和 title 的区别](https://github.com/lio-zero/blog/blob/main/HTML/alt%20%E5%92%8C%20title%20%E7%9A%84%E5%8C%BA%E5%88%AB.md)关于使用上需要注意的地方。

## accesskey

`accessKey` 属性规定激活（使元素获得焦点）当前元素的快捷键。

```html
<input accesskey="b" />
<a href="https://www.baidu.com/" accesskey="c">百度一下，你就知道</a>
```

注意，不同浏览器使用的快捷键方法不同：

- **IE，Chrome，Safari，Opera 15+**：`[ALT] + accesskey`
- **Opera prior version 15**：`[SHIFT] [ESC] + accesskey`
- **Firefox**：`[ALT] [SHIFT] + accesskey`

## tabindex

`tabindex` 属性规定当使用键盘上的 **tab** 键进行导航时元素的顺序。

在 HTML4.01 中，`tabindex` 属性可用于：`<a>`、`<area>`、`<button>`、`<input>`、`<object>`、`<select>` 和 `<textarea>`。

在 HTML5 中，`tabindex` 属性可用于任何的 HTML 元素（它会验证任何 HTML 元素，但不一定是有用）

```html
<ul>
  <li tabindex="2">HTML</li>
  <li tabindex="1">CSS</li>
  <li tabindex="3">JAVASCRIPT</li>
</ul>
```

## download

在锚定上使用时，`download` 属性表示浏览器应下载锚定指向的资源，而不是导航到该资源。

```html
<a href="/logo.png" download></a>
<!-- 下载的文件名为 'logo' -->
<a href="/logo.png" download="logo">logo</a>
```

详细内容可查看[使用 HTML5 download 属性创建可下载的链接](https://github.com/lio-zero/blog/blob/master/HTML/%E4%BD%BF%E7%94%A8%20HTML5%20download%20%E5%B1%9E%E6%80%A7%E5%88%9B%E5%BB%BA%E5%8F%AF%E4%B8%8B%E8%BD%BD%E7%9A%84%E9%93%BE%E6%8E%A5.md)。

## autocomplete

HTML `autocomplete` 属性为 `<input>` 字段提供了各种各样的选项。其作用是向浏览器指示值是否应在适当时自动填充。

```html
<input autocomplete="on|off" />
```

当要求用户创建新密码时，可以使用 `new-password` 值。此值可确保浏览器不会意外填写现有密码，同时还允许浏览器建议一个安全密码：

```html
<form action="signup" method="post">
  <input type="text" autocomplete="username" />
  <input type="password" autocomplete="new-password" />
  <input type="submit" value="注册" />
  <button type="reset">重置</button>
</form>
```

详细内容可查看 [HTML autocomplete 属性](https://github.com/lio-zero/blog/blob/main/HTML/HTML%20autocomplete%20%E5%B1%9E%E6%80%A7.md)。

## loading

我们经常需要对网站中的图像进行优化，其中一些技术便是懒加载，通常是延迟加载初始视口外的图像，直到我们滚动页面，达到图像与底部视口的交汇处才开始加载图像。

通常情况下，我们都是使用 JS 编写该方法。而 HTML 标准中已经存在 `loading` 属性，可以精确的感知这种行为。

我们只需要在想到延迟加载的图像上添加 `loading="lazy"` 属性即可：

```html
<img src="/img/logo.png" alt="website logo" loading="lazy" />
```

以下是 Can I Use 给出其的兼容情况：

![loading 支持情况](https://upload-images.jianshu.io/upload_images/18281896-ad5bc70865973a6f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**注意**：大多数现代浏览器都支持 `loading` 属性，但 Safari 和 IE 11 则不支持该属性。

详细内容可查看[使用 loading 属性延迟加载图片](https://github.com/lio-zero/blog/blob/master/HTML/%E4%BD%BF%E7%94%A8%20loading%20%E5%B1%9E%E6%80%A7%E5%BB%B6%E8%BF%9F%E5%8A%A0%E8%BD%BD%E5%9B%BE%E7%89%87.md)。

## 自定义有序列表的属性

### reversed 和 start

HTML `reversed` 属性可以帮助我们创建一个降序的编号列表。此布尔属性特定于 `<ol>` 元素，并指定列表元素的顺序相反（即从高到低编号）。

对列表顺序进行降序：

```html
<ol reversed>
  <li>item 3</li>
  <li>item 2</li>
  <li>item 1</li>
</ol>
<!--
 3. item 3
 2. item 2
 1. item 1
-->
```

`start` 和 `reversed` 属性一样，都用于有序列表 `<ol>` 元素，它的值为一个整数，用于指定列表计数器的初始值。两者结合可以指定任意的以哪个倒序数字开始。

例如，如果你想在一个反向的 3 项列表中显示数字 101 到 99，只需添加 `start="101"`。

```html
<ol reversed start="101">
  <li>item 101</li>
  <li>item 100</li>
  <li>item 99</li>
</ol>
<!--
item 101
item 100
item 99
-->
```

> [创建编号项目的降序列表](https://github.com/lio-zero/blog/blob/main/HTML/%E5%88%9B%E5%BB%BA%E7%BC%96%E5%8F%B7%E9%A1%B9%E7%9B%AE%E7%9A%84%E9%99%8D%E5%BA%8F%E5%88%97%E8%A1%A8.md)

### type 和 value

`value` 属性用于在特定列表项上指定自定义数字。

```html
<ol>
  <li>item 1</li>
  <li value="101">item 101</li>
  <li>item 102</li>
</ol>
<!--
item 1
item 101
item 102
-->
```

`type` 属性用于指定有序列表项目的编号类型，可能的值有 `a`、`A`、`i`、`I` 和 `1`。

```html
<ol type="a">
  <li>item 1</li>
  <li>item 2</li>
  <li>item 3</li>
</ol>
<!--
a. item 1
b. item 2
c. item 3
-->
```

## data-* 和 dataset

`data-*` 属性是 HTML5 中引入的，它们允许开发者在 HTML 元素中存储自定义数据。这些属性以 `data-` 为前缀，后面跟着自定义的名称。

这些属性可以应用于任何 HTML 元素。

```html
<div class="avatar" data-user="IU" data-avatar-type="circle" data-animateSpeed>
  <img src="avatar.png" alt="avatar" />
</div>
```

另外，针对这种自定义属性，HTML5 提供了 `dataset` 属性可以方便的获取或设置 `data-*` 自定义数据属性集。

```js
const avatar = document.querySelector('.avatar')

avatar.dataset.user === 'IU' // true
avatar.dataset.avatarType === 'circle' // true

avatar.dataset.animateSpeed = 4000

// 添加不存在的属性
avatar.dataset.id = 'user'
console.log(avatar.dataset)
/*
DOMStringMap {
  user: 'IU',
  avatarType: 'circle',
  animatespeed: '',
  animateSpeed: '4000',
  id: 'user'
}
*/
```

## autofocus

`autofocus` 属性是 HTML5 中引入的，用于指定一个元素在页面加载完成后自动获取焦点。这个属性可以让开发者让用户不用手动点击就可以直接输入，提升用户体验。

这个属性可以应用于 `<input>`、`<select>`、`<textarea>` 和 `<button>` 元素。

```html
<input type="text" autofocus />
```

当页面加载完成后，带有 `autofocus` 属性的元素将自动获得焦点。

需要注意的是，如果有多个元素有 `autofocus` 属性，只有第一个会获得焦点。也有一些浏览器不支持 `autofocus` 属性。

## spellcheck

`spellcheck` 属性是 HTML5 中引入的，用于指定输入字段是否需要拼写检查。

这个属性可以应用于 `<input>` 和 `<textarea>` 元素。它支持以下值:

- `true`（默认值）— 启用拼写检查
- `false` — 禁用拼写检查

```html
<input type="text" spellcheck="true" />
```

当启用拼写检查功能时，浏览器会检查用户输入的文本中是否有拼写错误。如果发现拼写错误，浏览器可能会在文本上显示错误提示。

当拼写检查用户键入的内容妨碍到你时，可以选择关闭它。

可以配合使用 `lang` 属性，用于检查使用哪种语言。

需要注意的是，不同的浏览器可能对拼写检查的支持程度不同，且拼写检查的语言可能需要在浏览器或系统设置中设置.

## datalist

`<datalist>` 标签是 HTML5 中引入的，用于为输入字段提供一组可选的值。这个标签可以让开发者为输入字段提供自动完成功能，让用户在输入过程中获得提示。

`<datalist>` 标签包含一组 `<option>` 标签，每个 `<option>` 标签都包含一个可选的值。

```html
<label for="course">选择学习课程：</label>
<input list="target" name="course" id="course" />
<datalist id="target">
  <option value="HTML" />
  <option value="CSS" />
  <option value="JavaScript" />
  <option value="Node" />
  <option value="Vue" />
  <option value="React" />
  <option value="Vite" />
</datalist>
```

使用 `input` 元素的 `list` 属性来绑定 `datalist`。

`<input>` 标签的 `list` 属性指向 `<datalist>` 标签的 `id` 属性。当用户在输入字段中输入文本时，浏览器会显示一个下拉列表，其中包含所有匹配的值。

## poster

[poster](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/video#attr-poster) 属性是 HTML5 中引入的，用于为 `<video>` 元素指定一个预览图像。这个属性可以改善用户体验，因为它可以在视频加载之前显示一张图像，而不是黑屏。

当视频加载完成后，预览图像会被视频内容替换。

```html
<video
  controls
  src="https://archive.org/download/BigBuckBunny_124/Content/big_buck_bunny_720p_surround.mp4"
  poster="https://peach.blender.org/wp-content/uploads/title_anouncement.jpg?x11217"
  width="620"
></video>
```

需要注意的是，如果视频没有加载或加载失败，预览图像将一直保留。

如果未指定该属性，则在视频第一帧可用之前不会显示任何内容（黑屏），当视频的第一帧可用时，将其作为海报（poster）帧来显示。

## inputmode

[`inputmode`](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/inputmode) 属性是 HTML5 新引入的一个全局的枚举属性，用于指定输入字段的键盘布局。这个属性可以让开发者提供不同类型的输入，例如数字、字母、符号等。

`inputmode` 属性可以应用于 `<input>` 和 `<textarea>` 元素。它支持以下值：

- `verbatim` — 使用标准键盘布局。
- `latin` — 使用字母键盘布局。
- `latin-name` — 使用字母键盘布局，带有大写字母。
- `latin-prose` — 使用字母键盘布局，带有标点符号。
- `full-width-latin` — 使用全宽字符键盘布局。
- `kana` — 使用日语 kana 键盘布局。
- `katakana` — 使用日语 katakana 键盘布局。
- `numeric` — 使用数字键盘布局。
- `tel` — 使用电话键盘布局。
- `email` — 使用电子邮件键盘布局。
- `url` — 使用 URL 键盘布局。

设置 `inputmode` 属性可以提供更好的用户体验，因为它可以让浏览器在输入字段上提供相应的键盘布局。例如，如果输入字段是用于输入电话号码的，则可以使用 `inputmode="tel"`，这样浏览器就会在输入字段上提供电话键盘布局。

```html
Tel: <input type="text" inputmode="tel" />
```

![Tel](https://upload-images.jianshu.io/upload_images/18281896-b0ff27275d97d1e2.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```html
Email: <input type="text" inputmode="email" />
```

![Email](https://upload-images.jianshu.io/upload_images/18281896-92574394b7031980.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## decoding

`<img>` 标签的 `decoding` 属性用于指定图像的解码方式。它可以有三个可能的值：

- `async` — 同步解码图像。在图像加载完成前阻塞页面的渲染。
- `sync` — 异步解码图像。图像加载完成之前继续渲染页面。
- `auto`（默认值）— 由浏览器自行决定。

```html
<img src="/path/to/image.png" alt="image" decoding="sync" />
```

当图像加载完成之前，浏览器可以使用预解码数据来渲染页面。这可以提高页面的加载速度，因为图像可以在后台加载，而不会阻塞页面的渲染。

- 使用 `decoding="async"` 时，如果图像在加载完成之前不能显示，则可以使用其他图像来替换它。这可以通过使用 `srcset` 和 `sizes` 属性来实现。
- 使用 `decoding="auto"` 时，浏览器根据图像大小和当前网络状况来决定是否使用异步解码。

需要注意的是这个属性不能保证图像总是使用异步解码，因为最终决策权还是在浏览器手中。

## enterkeyhint

[`enterkeyhint`](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/enterkeyhint) 属性是 HTML5 中新增的属性，它可以用来提示浏览器在输入框中按下回车键时应该采取什么操作。

```html
<input enterkeyhint="enter" />
```

该属性值可以是以下中的任意一个：

- `enter` 提示浏览器将输入的值提交或结束输入。
- `done` 与 `enter` 的意思类似。
- `go` 提示浏览器执行某种操作，例如搜索。
- `next` 提示浏览器将焦点转移到下一个输入框。
- `previous` 提示浏览器将焦点转移到上一个输入框。
- `search` 提示浏览器执行搜索操作。
- `send` 提示浏览器将输入的值发送给接收方。这个值通常用于即时通信应用，比如聊天工具或社交应用。

```html
Enter: <input type="text" enterkeyhint="enter">
```

在手机键盘中进行输入时，可以看到键盘的 **enter** 键显示不同的操作：

![Enter](https://upload-images.jianshu.io/upload_images/18281896-4d553c36c936255c.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```html
Next: <input type="text" enterkeyhint="next">
```

![Next](https://upload-images.jianshu.io/upload_images/18281896-5647efdadd1c1ee7.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```html
Search: <input type="text" enterkeyhint="search">
```

![Search](https://upload-images.jianshu.io/upload_images/18281896-33fa3998809df287.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 更多资料

> 需要注意，不同浏览器对以上介绍的属性支持程度可能不同，详细支持情况请查阅 MDN 或 Can I Use。

可以进一步阅读以下内容，其中或多或少都讲到了一些你可能没有使用过的 HTML 属性，例如表单上传目录的 `webkitdirectory` 属性，语言翻译 `translate` 属性等。

- [自动断字依赖于已定义的文档语言](https://github.com/lio-zero/blog/blob/main/HTML/%E8%87%AA%E5%8A%A8%E6%96%AD%E5%AD%97%E4%BE%9D%E8%B5%96%E4%BA%8E%E5%B7%B2%E5%AE%9A%E4%B9%89%E7%9A%84%E6%96%87%E6%A1%A3%E8%AF%AD%E8%A8%80.md)
- [前端文件上传](https://github.com/lio-zero/blog/blob/main/JavaScript/%E5%89%8D%E7%AB%AF%E6%96%87%E4%BB%B6%E4%B8%8A%E4%BC%A0.md)
- [确保输入字段只能上传图片](https://github.com/lio-zero/blog/blob/main/HTML/%E7%A1%AE%E4%BF%9D%E8%BE%93%E5%85%A5%E5%AD%97%E6%AE%B5%E5%8F%AA%E8%83%BD%E4%B8%8A%E4%BC%A0%E5%9B%BE%E7%89%87.md)
- [过滤文件输入的文件类型](https://github.com/lio-zero/blog/blob/main/HTML/%E8%BF%87%E6%BB%A4%E6%96%87%E4%BB%B6%E8%BE%93%E5%85%A5%E7%9A%84%E6%96%87%E4%BB%B6%E7%B1%BB%E5%9E%8B.md)
- [按时间间隔刷新页面（不使用 JavaScript）](https://github.com/lio-zero/blog/blob/main/HTML/%E6%8C%89%E6%97%B6%E9%97%B4%E9%97%B4%E9%9A%94%E5%88%B7%E6%96%B0%E9%A1%B5%E9%9D%A2%EF%BC%88%E4%B8%8D%E4%BD%BF%E7%94%A8%20JavaScript%EF%BC%89.md)
- [防止浏览器要求的翻译](https://github.com/lio-zero/blog/blob/main/HTML/%E9%98%B2%E6%AD%A2%E6%B5%8F%E8%A7%88%E5%99%A8%E8%A6%81%E6%B1%82%E7%BF%BB%E8%AF%91.md)
- [HTML translate 属性](https://github.com/lio-zero/blog/blob/main/HTML/HTML%20translate%20%E5%B1%9E%E6%80%A7.md)
- [10 useful HTML5 features, you may not be using](https://dev.to/atapas/10-useful-html5-features-you-may-not-be-using-2bk0)
- [HTML5 Feature Tips](https://html5-tips.netlify.app/) & [GitHub 地址](https://github.com/atapas/html-tips-tricks)
- [使用 srcset 的响应式图像](https://github.com/lio-zero/blog/blob/main/HTML/%E4%BD%BF%E7%94%A8%20srcset%20%E7%9A%84%E5%93%8D%E5%BA%94%E5%BC%8F%E5%9B%BE%E5%83%8F.md)
- [HTML iframe 标签](https://github.com/lio-zero/blog/blob/main/HTML/HTML%20iframe%20%E6%A0%87%E7%AD%BE.md)
