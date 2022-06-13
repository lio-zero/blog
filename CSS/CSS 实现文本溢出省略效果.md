# CSS 实现文本溢出省略效果

## 单行文本溢出

以下是截断单行长文本的 CSS 片段：

```css
.truncate {
  width: 250px;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}
```

- `overflow: hidden` — 文字长度超出限定宽度，则隐藏超出的内容
- `text-overflow: ellipsis` — 规定当文本溢出时，显示省略符号来代表被修剪的文本
- `white-space: nowrap` — 设置文字在一行显示，不能换行

## 多行文本溢出

使用 `-webkit-line-clamp` 可以设置多行超出显示省略号，下面的 CSS 声明将行数限制为 2：

```css
.truncate {
  display: -webkit-box;
  -webkit-line-clamp: 3;
  -webkit-box-orient: vertical;
  overflow: hidden;
  text-overflow: ellipsis;
}
```

> **注意**：`-webkit-line-clamp` 属性仅在具有 `display: -webkit-box` 和 `-webkit-box-orient: vertical` 声明时有效。

## 更多资料

[Line Clampin (Truncating Multiple Line Text)](https://css-tricks.com/line-clampin/) 给出了更多灵活的文本溢出方案。
