# nodeName 与 tagName 的区别

`nodeName` 和 `tagName` 是获取 HTML 节点名称的属性。

`tagName` 用于获取节点类型为 1 的元素节点的类型。对于属性、注释、文本等其他类型的节点，使用 `nodeName` 获取节点的名称。

让我们看看下面的例子，我们有一个简单的按钮：

```js
<button id='login'>Login</button>
```

HTML 元素的 `nodeName` 和 `tagName` 属性相同：

```js
const button = document.getElementById('login')
button.nodeType // 1
button.nodeName // 'BUTTON'
button.tagName // 'BUTTON'
```

让我们访问表示 `id` 属性的节点：

```js
const idNode = button.getAttributeNode('id')
idNode.nodeType // 2
idNode.nodeName // 'id'
idNode.tagName // undefined
```

以类似的方式，按钮文本节点为 `nodeName` 和 `tagName` 提供不同的结果：

```js
const content = button.firstChild
content.nodeType // 3
content.nodeName // '#text'
content.tagName // undefined
```
