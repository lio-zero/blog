# JavaScript 事件传播机制

JavaScript 采用异步事件驱动编程模型，与 HTML 的交互是通过[事件](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Building_blocks/Events#event_bubbling_and_capture)实现的。

事件驱动编程是一种涉及构建发送和接收事件的应用的范例。当程序发出事件时，程序通过运行任何注册到该事件和上下文的回调函数来响应，并将相关数据传递给该函数。

例如，监听元素的的 `click` 事件，其中在事件发生时运行回调函数。

```js
document.addEventListener('click', () => {
  // 此回调函数在用户单击文档时运行
})
```

当我们执行了特定的事件时，如在一个按钮上监听了 `click` 点击事件，当你按下按钮时，它将触发你给定的事件句柄/事件处理程序（也就是一个函数，其中执行一些 JS 语句）。**但这个触发过程是怎样的呢？**下面我们通过一些问题来看看本文要讲的事件的传播机制。

## 什么是事件流？

事件流是在网页上接收事件的顺序。当你单击嵌套在其他各种元素中的元素时，在你单击目标元素后，它会先触发目标元素上的 `click` 事件，在一层层往上触发事件，最终到达最顶层的 `window` 对象，这是浏览器默认的事件冒泡行为（IE 9+）。事件流有两种方式：

- 从外到内（事件捕获）
- 从内到外（事件冒泡）

以下示例都用该结构：

```html
<ul id="emoji">
  <li id="smile">😀</li>
  <li>😁</li>
  <li>😂</li>
  <li>🤣</li>
  <li>😃</li>
  <li>😄</li>
</ul>

<script>
  emoji.onclick = function (e) {
    console.log(e.target.innerHTML)
  }
</script>
```

尝试运行上面示例，你会发现，单击每个 `<li>` 标签，你会得到其中的每个 `emoji`。

> 你可以在这 👉[测试效果](https://codepen.io/lio-zero/pen/YzZLLqq)，配合下文进行阅读。

能得到这样的结果是因为，它使用了事件冒泡一个最重要的原理：**事件委托（event delegation）**，后面我们会介绍到。

## IE 中的事件流

这里要介绍一下 IE 6-8 中的事件流，IE 6-8 中的事件流和标准的 DOM 事件流有（独）所（领）不（风）同（骚），IE 的事件流只支持事件冒泡，不支持事件捕获。

IE 9 之前，它不支持 `addEventListener`，也就不能使用其第三个参数来表示是冒泡还是捕获，它提供了非标准的事件监听器 `attachEvent()` 方法。

在这个模型中，事件对象使用 `event.srcElement` 属性，而不是 `event.target`，但两者的效果相同。

```js
emoji.attachEvent('onclick', function (e) {
  var target = e.target || e.srcElement
  console.log(target.innerHTML)
})
```

## 什么是事件冒泡？

**事件冒泡（默认模式）**是事件传播的一种，其中事件首先在目标元素上触发，然后在同一嵌套层次结构中依次在目标元素的祖先（父代）上触发，直到到达最外层的 DOM 元素。

```js
emoji.onclick = () => {
  alert(11)
}

smile.onclick = () => {
  alert(22)
}
```

当你点击 `smile` 元素时，首先弹出 22，然后才是 11。同一层次结构的事件由内到外依次执行。

## 什么是事件捕获？

**事件捕获**也是事件传播的一种，其中事件首先被最外层的元素捕获，然后在同一嵌套层次结构中依次触发目标元素的后代（子元素），直到到达最里面的 DOM 元素。

当一个事件发生以后，它会在不同的 DOM 节点之间传播（propagation）。[DOM 事件](http://www.w3.org/TR/DOM-Level-3-Events/)标准描述了事件传播的 3 个阶段：

- **捕获阶段（capture phase）** — 事件从 `window` 对象向下传递到目标元素。
- **目标阶段（target phase）** — 事件到达目标元素。
- **冒泡阶段（bubbling phase）** — 事件从目标元素上开始冒泡。

在捕获阶段中，事件从祖先元素向下传播到目标元素。当事件达到目标元素后，冒泡才开始。

同时，这三个阶段的传播模型，会使得一个事件在多个节点上触发。

因为浏览器默认是从事件冒泡开始，我们看不到事件捕获，所以想要测试事件捕获，我们需要使用到 `addEventListener` 方法，`addEventListener` 用于添加事件句柄、注册监听器，第三个参数还可以指定在捕获阶段还是冒泡阶段触发事件：

- `false`（默认值）— 在冒泡阶段开始触发事件
- `true` — 在捕获阶段开始触发事件

```js
smile.addEventListener('click', (e) => {
  console.log(e.target.innerHTML)
})

emoji.addEventListener(
  'click',
  (e) => {
    alert('Emoji List')
  },
  true
)

document.addEventListener('click', () => {
  alert('document')
})

window.addEventListener('click', () => {
  alert('window')
})
```

运行上面代码，点击 `smile` 元素时，将依次输出 **'Emoji List' -> 😀 -> 'document' -> 'window'**。

**原因**：由于 `ul` 被我们设置为捕获阶段，所以它先被触发，然后在触发点击的目标元素事件，然后在依次往上冒泡。

> **注意**：捕获阶段不会在某些特殊事件（例如 `focus`）和 IE < 9 时出现。

## 事件委托（Event Delegation）

事件委托，也称为事件代理，是指将本要添加在自身的事件，添加到别人身上。通过冒泡的原理，将事件添加到父级，触发执行效果。

例如本文的示例，我们不必为列表的每个子项添加事件，而是添加在其父级上，当我们触发某一项时，通过事件冒泡到父级，从而触发事件。

```js
emoji.addEventListener('click', (e) => {
  console.log(e.target.innerHTML)
})
```

**好处**：

- 节省内存占用，减少事件注册
- 新增子元素时，无需再对其进行事件绑定

## 阻止事件传播

你可以通过调用事件的 `event.stopPropagation()` 方法来阻止捕获和冒泡阶段中当前事件在 DOM 中的进一步传播。

```js
function handler(e) {
  // do something...

  // 通常在事件处理程序的末尾
  e.stopPropagation()
}
```

## 阻止事件的默认行为

浏览器对某些事件操作有一些默认行为。例如：

- `click` 事件 — 点击元素时会触发该事件，默认行为是跳转到链接指向的页面。
- `submit` 事件 — 提交表单时会触发该事件，默认行为是提交表单数据到服务器。
- `keydown` 事件 — 按下键盘按键时会触发该事件，默认行为取决于按下的按键。例如，按下 `Enter` 键会提交表单，按下 `Tab` 键会使焦点跳到下一个输入字段。
- `dragstart` 事件 — 开始拖动元素时会触发该事件，默认行为是拖动元素。
- `drop` 事件 — 在拖动元素上释放鼠标时会触发该事件，默认行为是将拖动的数据放入目标元素中。

还有很多类型的事件，默认行为可能不同。为了确保页面的正常工作，如果在需要时，你可以使用以下方法来阻止默认行为的发生。

### 对于 `on<event>` 返回 `false`

```js
ele.onclick = function (e) {
  // do something

  return false
}
```

如果你使用内联属性，则情况相同：

```html
<form>
  <button type="submit" onclick="return false">Click</button>
</form>
```

我不推荐这种方法，因为：

- 返回 `false` 没有意义
- 它不适用于 `addEventListener()` 方法

### 使用 `preventDefault()` 方法

此方法适用于内联属性：

```html
<button type="submit" onclick="event.preventDefault()">Click</button>
```

和事件处理程序：

```js
ele.onclick = function (e) {
  e.preventDefault()
  // ...
}

ele.addEventListener('click', function (e) {
  e.preventDefault()
  // ...
})
```

你还可以在事件对象中使用 `event.defaultPrevented` 查看是否使用了 `event.preventDefault()` 方法。

它返回一个布尔值用来表明是否在特定元素中调用了 `event.preventDefault()`。

```js
function handler(e) {
  e.preventDefault()
  console.log(e.defaultPrevented) // true
}
```

## 很高兴你知道

- 事件本身在传播，而不是绑定在事件上的方法会传播。
- 并非所有的事件都会传播，如 `focus`、`blur`、`mouseenter` 和 `mouseleave` 等事件传播。
- 你应该知道哪些事件触发会有默认行为（如提交表单、点击链接等），并在需要时阻止它们。
- 建议使用 `addEventListener` 来注册事件处理程序，不要使用 HTML 元素的属性来注册事件处理程序（如 `onclick`），因为这样可能会有潜在的安全隐患。如果传递给 `onclick` 属性的方法不存在，浏览器也不会发出警告。
- 事件处理程序的返回值通常会被忽略，唯一的例外是使用 `on<event>` 分配的处理程序中返回 `return false`。在所有其他情况下，`return` 值都会被忽略，且返回 `true` 没有意义。
- 如果是使用 `on<event>` 分配的处理程序，那么你可以使用 `return false` 来阻止默认行为。
- 如果是使用 `addEventListener` 分配的处理程序，那么你可以使用 `event` 事件对象中的 `e.preventDefault()` 方法来阻止默认行为的发生。
- `return false` 做着与 `event.stopPropagation()` 和 `event.preventDefault()` 类似的工作，但它们之间毫无关联。
- 为了避免内存泄漏问题，请记住将不再使用的事件处理程序移除。

```js
const handler = (e) => {
  // do something...

  // 不在使用时，请移除它
  ele.removeEventListener('click', handler)
}

// 附加事件监听器
ele.addEventListener('click', handler)
```
