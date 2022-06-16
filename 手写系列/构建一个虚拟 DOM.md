# 构建一个虚拟 DOM

> 本文译自 [How to write your own Virtual DOM](https://medium.com/@deathmood/how-to-write-your-own-virtual-dom-ee74acc13060)，为了更好的理解 Virtual DOM 的基本概念，不增加其复杂性，它并没有设置 `props`、处理事件等，如果想要了解这些内容，可以查看第二部分 [Write your Virtual DOM 2: Props & Events](https://medium.com/@deathmood/write-your-virtual-dom-2-props-events-a957608f5c76)。

构建自己的虚拟 DOM 需要了解两件事。

- 虚拟 DOM 是真实 DOM 的任何一种表示形式
- 当我们在虚拟 DOM 树中更改某些内容时，我们会得到一个新的虚拟树。算法比较这两种树（旧树和新树），找出差异，并且只对真实 DOM 进行必要的小改动，以反映虚拟 DOM。

## DOM 树

首先，我们需要以某种方式将 DOM 树存储在内存中。我们可以用普通的 JS 对象来实现这一点。假设我们有这样一棵树：

```html
<ul class="list">
  <li>item 1</li>
  <li>item 2</li>
</ul>
```

用 JS 来表示如下：

```js
{
  type: 'ul',
  props: { class: 'list' },
  children: [
    { type: 'li', props: {}, children: ['item 1'] },
    { type: 'li', props: {}, children: ['item 2'] }
  ]
}
```

在这里你可以注意到两件事：

- 我们用如下对象表示 DOM 元素

```js
{ type: '...', props: { ... }, children: [ ... ] }
```

- 我们用纯 JS 字符串表示 DOM 文本节点

但以这种方式书写大树是相当困难的。所以我们需要编写一个辅助函数，这样我们就可以更容易理解它的结构：

```js
function h(type, props, ...children) {
  return { type, props, children }
}
```

现在我们可以这样编写 DOM 树：

```js
h('ul', { class: 'list' }, h('li', {}, 'item 1'), h('li', {}, 'item 2'))
```

这样看起来就干净很多了。但我们还可以走的更远，你听说过 JSX？

如果你[在这里](https://babeljs.io/docs/plugins/transform-react-jsx/)阅读官方 Babel JSX 文档，你就会知道，Babel 会转译这段代码：

```html
<ul className="list">
  <li>item 1</li>
  <li>item 2</li>
</ul>
```

变成这样：

```jsx
React.createElement(
  'ul',
  { className: 'list' },
  React.createElement('li', {}, 'item 1'),
  React.createElement('li', {}, 'item 2')
)
```

注意到任何相似之处吗？如果我们可以用我们的 `h(...)` 调用替换那些 `React.createElement(...)`，事实证明我们可以 — 通过使用 **jsx pragma**。我们只需要在源文件的顶部包含类似注释行：

```jsx
/** @jsx h */

<ul className=”list”>
  <li>item 1</li>
  <li>item 2</li>
</ul>
```

它实际上告诉 Babel，转译 jsx 而不是 `React.createElement`，为每个节点调用 `h` 函数。你可以用任何东西来代替 `h`。这将被转译。

因此，总结我之前所说的，我们将这样编写我们的 DOM：

```jsx
/** @jsx h */

const a = (
  <ul className='list'>
    <li>item 1</li>
    <li>item 2</li>
  </ul>
)
```

这将由 Babel 转换为以下代码：

```jsx
const a = h(
  'ul',
  { className: 'list' },
  h('li', {}, 'item 1'),
  h('li', {}, 'item 2')
)
```

当函数 `h` 执行时，它将返回纯 JS 对象，我们的虚拟 DOM 表示：

```js
const a = {
  type: 'ul',
  props: { className: 'list' },
  children: [
    { type: 'li', props: {}, children: ['item 1'] },
    { type: ' li', props: {}, children: ['item 2'] }
  ]
}
```

## 应用 DOM 表示

我们已经将 DOM 树表示为普通的 JS 对象，并具有了一个清晰的结构。

接下来，我们需要以某种方式从中创建一个真实的 DOM。因为我们不能只是将我们的虚拟 DOM 表示附加到 DOM 中。

在开始之前，我们需要先说明一些事情：

- 我将使用以 `$` 开头的真实 DOM 节点（元素，文本节点）编写所有变量，所以 `$parent` 将是真实的 DOM 元素
- 虚拟 DOM 表示将在名为 `node` 的变量中
- 和 React 一样，你只能有一个根节点，所有其他节点都在里面

这里，我们编写一个函数 `createElement(...)`，它将获取一个虚拟 DOM 节点并返回一个真实的 DOM 节点：

```js
function createElement(node) {
  if (typeof node === 'string') {
    return document.createTextNode(node)
  }
  return document.createElement(node.type)
}
```

因为我们可以有两个文本节点，即纯 JS 字符串和元素，它们都是 JS 对象类型，比如：

```js
{ type: '...', props: { ... }, children: [ ... ] }
```

因此，我们可以在这里传递虚拟文本节点和虚拟元素节点，这将起作用。

现在让我们考虑子节点，它们中的每一个也是文本节点或元素。所以也可以用 `createElement(...)` 函数来创建。

所以，我们可以为每个元素的子元素递归调用 `createElement(...)`，然后 `appendChild(...)` 将它们添加到我们的元素中，如下所示：

```js
function createElement(node) {
  if (typeof node === 'string') {
    return document.createTextNode(node)
  }
  const $el = document.createElement(node.type)
  node.children.map(createElement).forEach($el.appendChild.bind($el))
  return $el
}
```

效果如下：

![转为真实 DOM](https://upload-images.jianshu.io/upload_images/18281896-e2d321255bdc5f5c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 处理变化

现在我们可以把虚拟 DOM 变成真正的 DOM，是时候考虑区分我们的虚拟树了。我们需要编写一个算法，它将比较两个虚拟树（新旧树），并只对真实 DOM 进行必要的更改。

如何区分树？往下看一个示例：

- 没有旧节点，添加了新节点，我们需要 `appendChild(...)`

![image.png](https://upload-images.jianshu.io/upload_images/18281896-b008ea0d2a03d845.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- 没有新节点，因此旧节点需要被删除，我们需要 `removeChild(...)`

![image.png](https://upload-images.jianshu.io/upload_images/18281896-589b268386db6996.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- 有一个不同的节点，因此节点发生了变化，我们需要 `replaceChild(...)`

![image.png](https://upload-images.jianshu.io/upload_images/18281896-2f7513ec4cad719a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- 节点是相同的，所以我们需要更深入地区分子节点

![image.png](https://upload-images.jianshu.io/upload_images/18281896-40cb86119e36240e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

让我们编写一个名为 `updateElement(...)` 的函数，它接受三个参数：`$parent`、`newNode` 和 `oldNode`，其中 `$parent` 是一个真实的 DOM 元素，是虚拟节点的父元素。现在我们将了解如何处理上述所有情况。

### 没有旧节点

没有旧节点时，添加新节点：

```js
function updateElement($parent, newNode, oldNode) {
  if (!oldNode) {
    $parent.appendChild(createElement(newNode))
  }
}
```

### 没有新节点

这里我们有一个问题，如果在新的虚拟树的当前位置没有节点，我们应该从真实的 DOM 中删除它，但我们应该怎么做?

是的，我们知道父元素，因此我们应该调用 `$parent.removechild(...)`，并在那里传递真正的 DOM 元素引用。

但我们没有。如果我们知道我们的节点在 `parent` 中的位置，我们就可以通过 `$parent.childNodes[index]` 获取它的引用。其中 `index` 是节点在父元素中的位置。

```js
function updateElement($parent, newNode, oldNode, index = 0) {
  if (!oldNode) {
    $parent.appendChild(createElement(newNode))
  } else if (!newNode) {
    $parent.removeChild($parent.childNodes[index])
  }
}
```

### 节点已更改

首先，我们需要编写一个函数来比较两个节点（旧节点和新节点），并告诉我们节点是否真的发生了变化。我们应该考虑它既可以是元素也可以是文本节点：

```js
function changed(node1, node2) {
  return (
    typeof node1 !== typeof node2 ||
    (typeof node1 === 'string' && node1 !== node2) ||
    node1.type !== node2.type
  )
}
```

现在，有了当前节点在父节点中的索引，我们可以很容易地用新创建的节点替换它:

```js
function updateElement($parent, newNode, oldNode, index = 0) {
  if (!oldNode) {
    $parent.appendChild(createElement(newNode))
  } else if (!newNode) {
    $parent.removeChild($parent.childNodes[index])
  } else if (changed(newNode, oldNode)) {
    $parent.replaceChild(createElement(newNode), $parent.childNodes[index])
  }
}
```

### 不同的 children

最后，但并非最不重要的是，我们应该遍历这两个节点的每一个子节点并对它们进行比较，实际上为它们分别调用 `updateElement(...)`。是的，再次递归。

但是在编写代码之前，有一些事情需要考虑：

- 只有当节点是元素时才应该比较子节点（文本节点不能有子节点）
- 现在我们将当前节点的引用作为父节点传递
- 我们应该逐个比较所有的子函数，即使在某些时候我们会有 `undefined`，这是可以的，我们的函数可以处理
- 最后 `index` 是 `children` 数组中子节点的索引

```js
function updateElement($parent, newNode, oldNode, index = 0) {
  if (!oldNode) {
    $parent.appendChild(createElement(newNode))
  } else if (!newNode) {
    $parent.removeChild($parent.childNodes[index])
  } else if (changed(newNode, oldNode)) {
    $parent.replaceChild(createElement(newNode), $parent.childNodes[index])
  } else if (newNode.type) {
    const newLength = newNode.children.length
    const oldLength = oldNode.children.length
    for (let i = 0; i < newLength || i < oldLength; i++) {
      updateElement(
        $parent.childNodes[index],
        newNode.children[i],
        oldNode.children[i],
        i
      )
    }
  }
}
```

以上就是虚拟 DOM 的实现。

> 点击此处[查看示例](https://jsfiddle.net/deathmood/0htedLra/)
