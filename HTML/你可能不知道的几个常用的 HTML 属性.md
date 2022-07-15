# 你可能不知道的几个常用的 HTML 属性

## contenteditable

`contenteditable` 表示元素内容是否可被用户编辑。它可以有以下值：

- `true` 或者空字符串，表示元素是可被编辑的；
- `false` 表示元素不可被编辑。
- `inherit` 表示元素继承其父元素的可编辑状态

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

## dir

`dir` 属性规定元素内容的文本方向。它的取值如下：

- `ltr` —— 默认。从左向右的文本方向。常用于那种从左向右书写的语言（比如英语）；
- `rtl` —— 从右向左的文本方向。常用于那种从右向左书写的语言（比如阿拉伯语）；
- `auto` —— 让浏览器根据内容来判断文本方向。它在解析元素中字符时会运用一个基本算法，直到发现一个具有强方向性的字符，然后将这一方向应用于整个元素。仅在文本方向未知时推荐使用。

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

`hidden` 布尔属性表示该元素尚未或不再相关。

```html
<div hidden>lorem</div>
<div hidden="true">lorem</div>
<div hidden="false">lorem</div>
```

你可以配合 JS 来改变其值为 `true` 或 `false`，使浏览器是否需要渲染此类元素。例如，它可用于隐藏在登录过程完成之前无法使用的页面元素。

## title

HTML `title` 包含表示与其所属元素相关的建议信息的文本。也就是指定元素的提示文本。

```html
<p title="爱在这">爱在这</p>
```

当鼠标移动到带有 `title` 属性的元素上时，提示文本将作为工具提示（tooltip）显示出来。可以说，`title` 是对该元素的描述和进一步的说明。

可查看 [HTML title 属性](https://github.com/lio-zero/blog/blob/master/HTML/HTML%20title%20%E5%B1%9E%E6%80%A7.md) 关于使用上需要注意的地方。

## accesskey

`accessKey` 属性规定激活（使元素获得焦点）当前元素的快捷键。

```html
<input accesskey="b" />
<a href="https://www.baidu.com/" accesskey="c">百度一下，你就知道</a>
```

> **注意**：不同浏览器使用的快捷键方法不同：
>
> - **IE，Chrome，Safari，Opera 15+**：`[ALT] + accesskey`
> - **Opera prior version 15**：`[SHIFT] [ESC] + accesskey`
> - **Firefox**：`[ALT] [SHIFT] + accesskey`

## tabindex

- `tabindex` 属性规定当使用键盘上的 **tab** 键进行导航时元素的顺序。
- 在 HTML4.01 中，`tabindex` 属性可用于：`<a>`，`<area>`，`<button>`，`<input>`，`<object>`，`<select>` 和 `<textarea>`。
- 在 HTML5 中，`tabindex` 属性可用于任何的 HTML 元素（它会验证任何 HTML 元素。但不一定是有用）

```html
<ul>
  <li tabindex="2">HTML</li>
  <li tabindex="1">CSS</li>
  <li tabindex="3">JAVASCRIPT</li>
</ul>
```

## Download

在锚定上使用时，`Download` 属性表示浏览器应下载锚定指向的资源，而不是导航到该资源。

```html
<a href="/logo.png" download></a>
<!-- 下载的文件名为 'logo' -->
<a href="/logo.png" download="logo">logo</a>
```

详细可查看 [使用 HTML5 download 属性创建可下载的链接](https://github.com/lio-zero/blog/blob/master/HTML/%E4%BD%BF%E7%94%A8%20HTML5%20download%20%E5%B1%9E%E6%80%A7%E5%88%9B%E5%BB%BA%E5%8F%AF%E4%B8%8B%E8%BD%BD%E7%9A%84%E9%93%BE%E6%8E%A5.md)

## autocomplete

HTML `autocomplete` 属性为 `<input>` 字段提供了各种各样的选项。其作用是向浏览器指示值是否应在适当时自动填充。

```html
<input autocomplete="on|off" />
```

`new-password` —— 当要求用户创建新密码（例如，注册或重置密码输入框）时，可以使用该值。此值可确保浏览器不会意外填写现有密码，同时还允许浏览器建议一个安全密码：

```html
<form action="signup" method="post">
  <input type="text" autocomplete="username" />
  <input type="password" autocomplete="new-password" />
  <input type="submit" value="注册" />
  <button type="reset">重置</button>
</form>
```

详细可查看 [HTML autocomplete 属性](https://github.com/lio-zero/blog/blob/master/HTML/HTML%20autocomplete%20%E5%B1%9E%E6%80%A7.md)

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

详细可查看[使用 loading 属性延迟加载图片](https://github.com/lio-zero/blog/blob/master/HTML/%E4%BD%BF%E7%94%A8%20loading%20%E5%B1%9E%E6%80%A7%E5%BB%B6%E8%BF%9F%E5%8A%A0%E8%BD%BD%E5%9B%BE%E7%89%87.md)。

## reversed 和 start

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

`start` 属性和 `reversed` 一样，都用于有序列表 `<ol>` 元素，它的值为一个整数，用于指定列表计数器的初始值。两者结合可以指定任意的以哪个倒序数字开始。

例如，如果您想在一个反向的 3 项列表中显示数字 101 到 99，只需添加 `start="101"`。

```html
<ol reversed start="101">
  <li>item 101</li>
  <li>item 100</li>
  <li>item 99</li>
</ol>
```

## data-\* 和 dataset

`data-*` 自定义数据属性，它赋予我们在所有 HTML 元素上嵌入自定义数据属性的能力。

```html
<div class="avatar" data-user="IU" data-avatar-type="circle" data-animateSpeed>
  <img src="avatar.png" alt="avatar" />
</div>
```

然后，你可以通过 JS 与 HTML 之间进行专有数据的交互。通过 `elem.dataset` 可以方便的获取或设置 `data-*` 自定义数据属性集。

```js
const avatar = document.querySelector('.avatar')

console.log(avatar.dataset.user === 'IU') // true
console.log(avatar.dataset.avatarType === 'circle') // true
avatar.dataset.animateSpeed = 4000

// 添加不存在的属性
avatar.dataset.id = 'user'
console.log(avatar.dataset)
```

## autofocus

`autofocus` 属性用于自动对焦：

```html
<input autofocus />
```

当页面加载时将焦点放在指定的 HTML 元素上。

## spellcheck

`spellcheck` 属性定义是否可以检查元素的拼写错误。它可以具有以下值：

```html
<input type="text" spellcheck="true|false" />
```

当拼写检查用户键入的内容妨碍到您时，可以选择关闭它。

## datalist

`<datalist>` 标签定义选项列表。与 `input` 元素配合使用，来定义 `input` 可能的值。

自动建议文本输入控件：

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
  <option value="Vite"></option>
</datalist>
```

使用 `input` 元素的 `list` 属性来绑定 `datalist`。

## 更多资料

[HTML5 Feature Tips](https://html5-tips.netlify.app/) & [地址](https://github.com/atapas/html-tips-tricks)
