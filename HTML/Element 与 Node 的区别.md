# Element 与 Node 的区别

一个网页是由一个节点树构成的。考虑到常见 HTML 页面的结构如下：

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body></body>
</html>
```

树的根节点是一个文档节点，它有两个子节点，`doctype` 和 `html` 标签。`html` 标签由其子项 `head` 和 `body` 等构成。

每个节点都有一个名为 `nodeType` 的属性，该属性是标识其类型的数字。它可以用来区分节点的类型。例如，以下 `div` 只有一个子节点，其值为 `Hello`：

```html
<div id="ele">Hello</div>
```

`div` 节点是元素节点，而其子节点 `Hello` 是文本节点。下表取自 [MDN](https://developer.mozilla.org/en-US/docs/Web/API/Node/nodeType)，显示了常用的节点类型：

- `Node.ELEMENT_NODE` (`1`) — 一个元素节点，它是一个 HTML 标签，如 `<div>`、`<p>` 等。
- `Node.TEXT_NODE` (`3`) — 元素节点内的实际文本。
- `Node.COMMENT_NODE` (`8`) — 注释节点，例如 `<!-...-->`。
- `Node.DOCUMENT_NODE` (`9`) — 文档节点。
- `Node.DOCUMENT_TYPE_NODE` (`10`) — 文档类型节点，例如 `<!DOCTYPE html>`。
- etc

```js
document.nodeType // 9
```

**简而言之，Node 表示文档树中的单个节点，Element 是一种特定类型的节点，即 HTML 标签。**
