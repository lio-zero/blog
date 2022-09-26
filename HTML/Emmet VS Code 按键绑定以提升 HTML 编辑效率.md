# Emmet VS Code 按键绑定以提升 HTML 编辑效率

Emmet 是一个自动将代码片段扩展为 HTML 的工具。它包含在 VS Code 中。

例如以下片段：

```txt
div.someClass>span*5
```

将展开为：

```html
<div class="someClass">
  <span></span>
  <span></span>
  <span></span>
  <span></span>
  <span></span>
</div>
```

Emmet 还提供了其他一些快捷方式提升 HTML 开发效率。

> 推荐：[Emmet 语法](https://github.com/lio-zero/blog/blob/master/HTML/Emmet%20%E8%AF%AD%E6%B3%95.md)

## 添加 VS Code 快捷方式

> 组合键：`Ctrl + K` 和 `Ctrl + S` 打开键盘快捷键窗口，在搜索框输入 Emmet，可以找出内置 Emmet 可以绑定热键的特定操作。

按住 `Ctrl + Shift + p` 打开命令面板，输入 `shortcut`，找到**打开键盘快捷键方式**的选项。

将打开一个按键绑定的 `keybindings.json` 文件：

```json
[]
```

每个添加的自定义快捷方式都反映在此文件中，并具有以下结构：

```json
{
  "key": "<key combination>",
  "command": "<command to run>"
}
```

## VS Code 中可用的 Emmet 命令

Emmet 的可用命令如下：

```txt
editor.emmet.action.balanceIn
editor.emmet.action.balanceOut
editor.emmet.action.decrementNumberByOne
editor.emmet.action.decrementNumberByOneTenth
editor.emmet.action.decrementNumberByTen
editor.emmet.action.evaluateMathExpression
editor.emmet.action.incrementNumberByOne
editor.emmet.action.incrementNumberByOneTenth
editor.emmet.action.incrementNumberByTen
editor.emmet.action.matchTag
editor.emmet.action.mergeLines
editor.emmet.action.nextEditPoint
editor.emmet.action.prevEditPoint
editor.emmet.action.reflectCSSValue
editor.emmet.action.removeTag
editor.emmet.action.selectNextItem
editor.emmet.action.selectPrevItem
editor.emmet.action.splitJoinTag
editor.emmet.action.toggleComment
editor.emmet.action.updateImageSize
editor.emmet.action.updateTag
editor.emmet.action.wrapIndividualLinesWithAbbreviation
editor.emmet.action.wrapWithAbbreviation
```

以下是其中的部分示例。我们使用 `alt + e` 和 `alt + *` 组合，按键可能会因为系统和其他软件冲突，调整到自己舒服即可。

**平滑向内/平滑向外** — 从当前插入符号位置搜索标签或标签的内容边界并选择它。

```json
[
  {
    "key": "alt+e alt+i",
    "command": "editor.emmet.action.balanceIn"
  },
  {
    "key": "alt+e alt+o",
    "command": "editor.emmet.action.balanceOut"
  }
]
```

**转到配对标签** — 在开始和结束元素标签之间跳转。

```json
[
  {
    "key": "alt+e alt+e",
    "command": "editor.emmet.action.matchTag"
  }
]
```

**删除标签** — 从 HTML 树中删除标签但保留其内部 HTML。

```json
[
  {
    "key": "alt+e alt+d",
    "command": "editor.emmet.action.removeTag"
  }
]
```

另外，如果你不想自己配置热键，可以安装 [Emmet Keybindings](https://marketplace.visualstudio.com/items?itemName=agutierrezr.emmet-keybindings) 扩展，它是一组用于 VS Code 的 Emmet 键绑定。它可以用作预定义的键绑定组，以防您不知道映射到哪个键。

## 更多资料

还有许多有用的缩写，例如 [Wrap with Abbreviation](https://docs.emmet.io/actions/wrap-with-abbreviation/) 和 [Remove Tag](https://docs.emmet.io/actions/remove-tag/) ，查阅它们以了解更多。
