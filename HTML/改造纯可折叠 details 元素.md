# 改造纯可折叠 details 元素

我们可以创建一个带有纯 HTML 标签的可扩展元素，如下所示：

```html
<details>
  <summary>Details</summary>
  <!-- 单击 summary 标签时显示的隐藏内容。 -->
  Something small enough to escape casual notice.
</details>
```

[`<details>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/details) 元素可创建一个挂件，仅在被切换成展开状态时，它才会显示内含的信息。[`<summary>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/summary) 元素可为该部件提供概要或者标签。

在没有添加任何样式的情况下，效果如下：

![纯可折叠元素.gif](https://upload-images.jianshu.io/upload_images/18281896-b3816915c4ac3922.gif?imageMogr2/auto-orient/strip)

我们给它加一点样式：

```css
details {
  border: 1px solid #aaa;
  border-radius: 4px;
  padding: 0.5em 0.5em 0;
  background: #282828;
  color: #fff;
}

summary {
  display: block;
  background: #333;
  font-weight: bold;
  margin: -0.5em -0.5em 0;
  padding: 1rem;
  padding-left: 2.2rem;
  position: relative;
  cursor: pointer;
}

summary:before {
  content: '';
  border-width: 0.4rem;
  border-style: solid;
  border-color: transparent transparent transparent #fff;
  position: absolute;
  top: 1.3rem;
  left: 1rem;
  transform: rotate(0);
  transform-origin: 0.2rem 50%;
  transition: 0.25s transform ease;
}

details[open] > summary:before {
  transform: rotate(90deg);
}

details summary::-webkit-details-marker {
  display: none;
}

details[open] > summary::before {
  transform: rotate(90deg);
}

details summary::-webkit-details-marker {
  display: none;
}

summary:focus {
  outline: none;
  box-shadow: none;
}

details[open] {
  padding: 0.5em;
}

details[open] summary {
  border-bottom: 1px solid #aaa;
  margin-bottom: 0.5em;
}
```

在给它来点动画效果：[How to Animate the Details Element](https://css-tricks.com/how-to-animate-the-details-element/)

效果如下：

![纯可折叠元素](https://upload-images.jianshu.io/upload_images/18281896-2f95bdf9149d4db0.gif?imageMogr2/auto-orient/strip)
