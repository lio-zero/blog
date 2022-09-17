# Markdown 语法

Markdown 是一种轻量级且易于使用的语法，它允许人们使用易读易写的纯文本格式编写文档。其文件扩展名为 `.md` 或 `.markdown`。

当前许多网站都广泛使用 Markdown 来撰写帮助文档或是用于论坛上发表消息。例如：GitHub、reddit、掘金和知乎等平台。

本文使用 [Typora](https://typora.io/) 编写，它支持 macOS 、Windows、Linux 平台，且包含多种主题，编辑后直接渲染出效果。支持导出 HTML、PDF、Word、图片等多种类型文件。

> **Tips**：这篇文章是在 2021 年上半年撰写完成的。期间 Typora 已转为收费项目，现在我使用 VS Code 记录日常学到的知识。
>
> 另外，有一些 markdown 语法在某些网站可能无法成效，这取决于他们网站的实现，与您编写的 markdown 语法无关。您可以下载一些有名的 markdown 编辑器或其他在线编辑器进行学习。推荐一篇我之前整理的文章：[分享六款免费、优质 Markdown 编辑器](https://zhuanlan.zhihu.com/p/362791233)。

下面开始讲解 markdown 语法，可能不全，最后会提供一些学习 markdown 语法的网站。

## 标题

```markdown
# h1

## h2

### h3

#### h4

##### h5

###### h6

# Header 1

## Header 2
```

效果：

![标题](https://upload-images.jianshu.io/upload_images/18281896-524bfe826d774316.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

为了不影响排版，这里使用图片。

## 强调

```markdown
_italic_
_italic_
**bold**
**bold**
`code`
```

效果：

_italic_
_italic_
**bold**
**bold**
`code`

## 列表

**无序列表**使用星号（**\***）、加号（**+**）或是减号（**-**）作为列表标记

```markdown
- Item 1
- Item 2

* Item 1
* Item 2

- Item 1
- Item 2
```

效果：

- Item 1
- Item 2

- Item 1
- Item 2

- Item 1
- Item 2

**有序列表**使用数字并加上 **.** 号来表示

```markdown
1. Item 1
2. Item 2
```

效果：

1. Item 1
2. Item 2

**列表嵌套**：

```markdown
- Item 1
  - Item 1
```

效果：

- Item 1
  - Item 1

## 清单

**待办事宜 Todo 列表**：

```markdown
- [ ] Update the website
- [x] Write the press release
```

效果：

- [ ] Update the website
- [x] Write the press release

![任务列表](https://upload-images.jianshu.io/upload_images/18281896-93e4a024a29bf5ed.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 链接

链接使用方法如下：

```markdown
[Link name](path)

<path>
```

效果：

[link](https://github.com/lio-zero)

[https://github.com/lio-zero](https://github.com/lio-zero)

**添加标题**：

```markdown
[Link name](https://github.com/lio-zero 'title')
```

鼠标悬停链接上，查看效果：

[我的 GitHub](https://github.com/lio-zero '我的 GitHub')

**其他操作**：

通过变量来设置一个链接，变量赋值在文档末尾进行：

```markdown
[lio][blog]
[blog]: https://github.com/lio-zero
```

效果：

![通过变量来设置一个链接](https://upload-images.jianshu.io/upload_images/18281896-e3a17cbc5f73d676.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

一些编辑器所呈现的效果跟 typora 不一样，这里直接贴图片。

## 图片

```markdown
![Image alt text](/path/to/img.jpg)
```

![显示图片](https://upload-images.jianshu.io/upload_images/18281896-66e750497d771bc2.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**添加标题**：

```markdown
![Image alt text](img.jpg 'title')
```

![添加标题](https://upload-images.jianshu.io/upload_images/18281896-923779045965ebc2.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**直接使用 `<img />` 标签**

```html
<img src="path" />
```

如果在 GitHub 上使用图片，您还可以：

```markdown
![GitHub Logo](/images/logo.png)
```

将当前的 URL 拼接在 `/images/logo.png` 前面。不只是图片，其他链接也是如此。

> 在这一节先说一下，您可以直接在 markdown 上使用 HTML 标签，在使用内联样式进行美化。

## 语法高亮

使用 **```** 包裹一段代码，并指定一种语言（也可以不指定）。

以下是使语法突出显示的示例：

![代码](https://upload-images.jianshu.io/upload_images/18281896-579edb2101b09601.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

效果：

```markdown
code fences
```

```js
console.log('代码块')
```

如果您有两段相同的代码，只是某一行可能存在差异，那么可以使用 `diff`，这在 GitHub 上尤其有用。

```diff
- console.log('Hello');
+ console.log('Hello')
```

## 内联代码

两个反引号 **``** 可以实现代码高亮效果：

```markdown
`==`（相等运算符）和 `===`（严格相等运算符）
```

效果：

`==`（相等运算符）和 `===`（严格相等运算符）

## 区块引用

Markdown 区块引用是在段落开头使用 **>** 符号 ，然后后面紧跟一个**空格**符号

```markdown
> 这是一个区块引用
```

效果：

> 这是一个区块引用

另外区块是可以嵌套的，一个 **>** 符号是最外层，两个 **>** 符号是第一层嵌套，以此类推

```markdown
> 最外层
>
> > 第一层嵌套
```

效果：

> 最外层
>
> > 第一层嵌套

## 分割线

使用三个以上的星号、减号、底线生成一个分隔线

```markdown
---
---

---
```

效果：

---

---

---

## 删除线

使用两个波浪线 **~~** 来添加删除线

```markdown
~~该内容已不再最新版本中使用~~
```

效果：

~~该内容已不再最新版本中使用~~

## 下划线

```markdown
<u>带下划线文本</u>
```

效果：

<u>带下划线文本</u>

## 脚注

脚注是对文本的补充说明。

```markdown
一段文字[^info]
```

效果：

![脚注](https://upload-images.jianshu.io/upload_images/18281896-27ee241c04c21fbc.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 内容目录

```markdown
[TOC]
```

[TOC]

![内容目录](https://upload-images.jianshu.io/upload_images/18281896-20acdcd94067a4d0.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

有些网站不支持这种模式，渲染效果会有问题，直接贴图片。

## 表格

Markdown 制作表格使用 **|** 来分隔不同的单元格，使用 **-** 来分隔表头和其他行。

```markdown
| Column 1 Heading | Column 2 Heading |
| ---------------- | ---------------- |
| Some content     | Other content    |

| Column 1 Heading | Column 2 Heading |
| ---------------- | ---------------- |
| Some content     | Other content    |
```

效果：

| Column 1 Heading | Column 2 Heading |
| ---------------- | ---------------- |
| Some content     | Other content    |

| Column 1 Heading | Column 2 Heading |
| ---------------- | ---------------- |
| Some content     | Other content    |

**对齐方式：**

- **-:** 设置内容和标题栏居右对齐。
- **:-** 设置内容和标题栏居左对齐。
- **:-:** 设置内容和标题栏居中对齐。

```markdown
| Column 1 Heading | Column 2 Heading | Column 2 Heading |
| :--------------- | :--------------: | ---------------: |
| Some content     |  Other content   |    Other content |
```

效果：

| Column 1 Heading | Column 2 Heading | Column 2 Heading |
| :--------------- | :--------------: | ---------------: |
| Some content     |  Other content   |    Other content |

## 使用表情符号短代码 🌐

使用开始和结束与冒号 **:**，并包括表情符号的名称。

```markdown
Gone camping! :tent: Be back soon.

That is so funny! :joy:
```

效果：

Gone camping! ⛺️ Be back soon.

That is so funny! 😂

你可以在下面这些网站上找到所有可用的 Emoji：

- [emojipedia](https://emojipedia.org/)
- [gist:7360908](https://gist.github.com/rxaviers/7360908)
- [markdown-it-emoji](https://github.com/markdown-it/markdown-it-emoji/blob/master/lib/data/full.json)
- [emoji-cheat-sheet](https://github.com/ikatyang/emoji-cheat-sheet)

## 最后

如果您在 VS Code 上撰写文章的话，可以安装以下扩展：

- [Markdown All in One](https://marketplace.visualstudio.com/items?itemName=yzhang.markdown-all-in-one)
- [Markdown Preview Enhanced](https://marketplace.visualstudio.com/items?itemName=yzhang.markdown-all-in-one)
- [markdownlint](https://marketplace.visualstudio.com/items?itemName=shd101wyy.markdown-preview-enhanced)
- [:emojisense:](https://marketplace.visualstudio.com/items?itemName=bierner.emojisense)

## 更多资料

- [Markdown Guide](https://www.markdownguide.org/)
- [Writing on GitHub](https://docs.github.com/en/get-started/writing-on-github)
- [Typora 画流程图、时序图(顺序图)、甘特图](https://www.runoob.com/markdown/md-advance.html) 来自菜鸟教程的 Markdown 高级技巧中的一篇笔记
