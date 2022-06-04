# 浏览器重绘和回流（Repaint & Reflow）

重排和重绘是关键渲染路径中的两步，本文将介绍它们两个的影响。

## 重绘（Repaint）

当对元素所做的更改明显改变其外观但不影响其布局时，将发生重绘。

这方面的例子包括 `outline`，`visibility`，`background`，或 `color`。[根据 Opera 的说法](https://dev.opera.com/articles/efficient-javascript/?page=3#reflow)，重绘是昂贵的，因为浏览器必须验证 DOM 树中所有其他节点的可见性。

您可以在 [CSS Triggers](http://csstriggers.com/) 上了解哪些 CSS 属性会引起回流、重绘和合成。

![CSS Triggers](https://upload-images.jianshu.io/upload_images/18281896-6cf44c322592a46a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

> **扩展**：重绘是在关键渲染路径中的 Paint 阶段，将渲染树中的每个节点转换成屏幕上的实际像素，这一步通常称为绘制或栅格化。

> **记住一点：回流必定会发生重绘，重绘不一定会引发回流。**

## 回流（Reflow）

元素的位置发生变动时将发生回流。

回流对性能更为关键，因为它涉及到影响部分页面（或整个页面）布局的更改。

导致回流的示例包括：添加或删除 DOM、显式或隐式改变 `width`、`height`、`font-family`、`font-size` 等。

> **扩展**：回流是在关键渲染路径中的 Layout 阶段，计算每一个元素在设备视口内的确切位置和大小。当一个元素位置发生变化时，其父元素及其后边的元素位置都可能发生变化，代价极高。

## 如何减少回流和重绘？

如何减少回流和重绘，可以查阅[介绍下重绘和回流（Repaint & Reflow），以及如何进行优化](https://github.com/Advanced-Frontend/Daily-Interview-Question/issues/24)。

## 更多资料

- [你真的了解回流和重绘吗](https://zhuanlan.zhihu.com/p/52076790)
- [前端性能优化：细说浏览器渲染的重排与重绘](http://www.imooc.com/article/45936)
- [ON LAYOUT & WEB PERFORMANCE](https://kellegous.com/j/2013/01/26/layout-performance/)
- [REFLOWS & REPAINTS: CSS PERFORMANCE MAKING YOUR JAVASCRIPT SLOW?](http://www.stubbornella.org/content/2009/03/27/reflows-repaints-css-performance-making-your-javascript-slow/)
