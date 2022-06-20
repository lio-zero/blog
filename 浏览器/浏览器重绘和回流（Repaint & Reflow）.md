# 浏览器重绘和回流（Repaint & Reflow）

重排和重绘是[关键渲染路径](https://developer.mozilla.org/zh-CN/docs/Web/Performance/Critical_rendering_path)中的两步，本文将介绍它们两个的影响。

## 重绘（Repaint）

当对元素所做的更改明显改变其外观但不影响其布局时，将发生重绘。

这方面的例子包括 `outline`，`visibility`，`background`，或 `color`。[根据 Opera 的说法](https://dev.opera.com/articles/efficient-javascript/?page=3#reflow)，重绘是昂贵的，因为浏览器必须验证 DOM 树中所有其他节点的可见性。

> **扩展**：重绘是在关键渲染路径中的 Paint 阶段，将渲染树中的每个节点转换成屏幕上的实际像素，这一步通常称为绘制或栅格化。

> **记住一点：回流必定会发生重绘，重绘不一定会引发回流。**

## 回流（Reflow）

元素的位置发生变动时将发生回流。

回流对性能更为关键，因为它涉及到影响部分页面（或整个页面）布局的更改。

导致回流的示例包括：添加或删除 DOM、显式或隐式改变 `width`、`height`、`font-family`、`font-size` 等。

> **扩展**：回流是在关键渲染路径中的 Layout 阶段，计算每一个元素在设备视口内的确切位置和大小。当一个元素位置发生变化时，其父元素及其后边的元素位置都可能发生变化，代价极高。

## 合成（Composite）

这节，额外介绍一下合成。

您可能听过 GPU 加速这个优化性能的方法，它其实是通过合成层来实现的。

例如 CSS 的 `will-change`、`transform`、`opacity` 等属性可以进入合成层实现 GPU 加速。

### 提升为合成层好处

在合成的情况下，会直接跳过布局（layout）和绘制（paint）阶段，直接进入非主线程处理的部分，即直接交给合成线程处理。

它有以下几个好处：

- 合成层的位图，会交由 GPU 合成，它会比 CPU 处理要快
- 当需要 repaint 时，只需要 repaint 本身，不会影响到其他的层
- 元素提升为合成层后，`transform` 和 `opacity` 才不会触发 paint，如果不是合成层，则其依然会触发 paint。

您可以在 [CSS Triggers](http://csstriggers.com/) 上了解哪些 CSS 属性会引起回流、重绘和合成。

![CSS Triggers](https://upload-images.jianshu.io/upload_images/18281896-6cf44c322592a46a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

> Tips：CSS Triggers 已无维护。

## 如何减少回流和重绘？

如何减少回流和重绘，可以查阅[介绍下重绘和回流（Repaint & Reflow），以及如何进行优化](https://github.com/Advanced-Frontend/Daily-Interview-Question/issues/24)。

## 更多资料

- [你真的了解回流和重绘吗](https://zhuanlan.zhihu.com/p/52076790)
- [前端性能优化：细说浏览器渲染的重排与重绘](http://www.imooc.com/article/45936)
- [ON LAYOUT & WEB PERFORMANCE](https://kellegous.com/j/2013/01/26/layout-performance/)
- [REFLOWS & REPAINTS: CSS PERFORMANCE MAKING YOUR JAVASCRIPT SLOW?](http://www.stubbornella.org/content/2009/03/27/reflows-repaints-css-performance-making-your-javascript-slow/)
- [详谈层合成（composite）](https://juejin.cn/post/6844903502678867981)
- [CSS GPU Animation: Doing It Right](https://www.smashingmagazine.com/2016/12/gpu-animation-doing-it-right/)
- [Accelerated Rendering in Chrome](https://web.dev/speed-layers/)
