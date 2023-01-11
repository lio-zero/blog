# mouseenter 与 mouseover 的区别

将鼠标移到元素或其子元素上时，将触发 `mouseenter` 和 `mouseover` 事件。但是它们之间有一些细微的差别：

- `mouseenter` 事件仅在鼠标移入元素时触发，而不包括移入元素的子元素。对应的鼠标离开元素事件是 `mouseleave`。
- `mouseover` 事件在鼠标移入元素或其任何子元素时触发，包括移入元素的子元素。对应的鼠标离开元素事件是 `mouseout`。

## 建议

因为 `mouseover` 事件是从子元素向上通过层次结构传播的，所以如果你正在对该事件执行资源密集型任务，你可能会注意到屏幕闪烁。

**建议改用 `mouseenter` 和 `mouseleave`事件。**
