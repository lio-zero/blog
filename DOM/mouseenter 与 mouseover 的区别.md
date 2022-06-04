# mouseenter 与 mouseover 的区别

将鼠标移到元素上时，将触发 `mouseenter` 和 `mouseover` 事件。

## 区别

- `mouseenter` 仅在鼠标进入设置它的元素时触发（不会冒泡）。对应的事件是 `mouseleave`。
- 当鼠标进入元素或其任何子元素时，`mouseover` 将触发（存在冒泡）。它的对应事件是 `mouseout`。

## 建议

因为 `mouseover` 事件是从子元素向上通过层次结构传播的，所以如果您正在对该事件执行资源密集型任务，您可能会注意到屏幕闪烁。

**建议改用 `mouseenter` 和 `mouseleave`事件。**
