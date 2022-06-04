# 在指定的 DOM 元素中渲染给定的 DOM 树

- 将第一个参数分解为 `type` 和 `props`。使用 `type` 确定给定元素是否为文本元素。
- 根据元素的 `type`，使用 `Document.createTextNode()` 或 `Document.createElement()` 创建 DOM 元素。
- 根据需要，使用 `Object.keys()` 向 DOM 元素添加属性并设置事件监听器。
- 使用递归渲染 `props.children`（如果有）。
- 最后，使用 `Node.appendChild()` 将 DOM 元素附加到指定的容器中。

```js
const renderElement = ({ type, props = {} }, container) => {
  const isTextElement = !type
  const element = isTextElement
    ? document.createTextNode('')
    : document.createElement(type)

  const isListener = (p) => p.startsWith('on')
  const isAttribute = (p) => !isListener(p) && p !== 'children'

  Object.keys(props).forEach((p) => {
    if (isAttribute(p)) element[p] = props[p]
    if (!isTextElement && isListener(p))
      element.addEventListener(p.toLowerCase().slice(2), props[p])
  })

  if (!isTextElement && props.children && props.children.length)
    props.children.forEach((childElement) =>
      renderElement(childElement, element)
    )

  container.appendChild(element)
}
const myElement = {
  type: 'button',
  props: {
    type: 'button',
    className: 'btn',
    onClick: () => alert('Clicked'),
    children: [{ props: { nodeValue: 'Click me' } }]
  }
}

renderElement(myElement, document.body)
```
