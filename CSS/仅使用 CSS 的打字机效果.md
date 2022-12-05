# 仅使用 CSS 的打字机效果

我们可以在完全不使用 JavaScript 的情况下，仅使用 CSS 一些小技巧便可以完成打字机效果。

我们可以使用 `white-space: nowrap` 和 `overflow: hidden` 来隐藏溢出字符，定义等宽字体以使容器的宽度可预测，然后设置周围容器宽度的动画。闪烁的光标通过设置右边框的动画来实现。

定义两个动画，`typing` 以设置角色动画和 `blink` 动画插入符号。

```html
<div class="type-writer">
  <div class="text">All work and no play makes Jack a dull boy!</div>
</div>
```

CSS 如下：

```css
.type-writer .text {
  width: 43ch;
  animation: typing 2s steps(22), blink 0.5s step-end infinite alternate;
  white-space: nowrap;
  overflow: hidden;
  border-right: 3px solid;
  font-family: monospace;
  font-size: 1.5em;
  margin: 1em auto;
}

@keyframes typing {
  from {
    width: 0;
  }
}

@keyframes blink {
  50% {
    border-color: transparent;
  }
}
```

👇 以下的输入是在没有 JavaScript 的情况下完成的！

![打字机效果](https://upload-images.jianshu.io/upload_images/18281896-83c926ba8338b7f6.gif?imageMogr2/auto-orient/strip)

你可能需要注意的是，你要根据你内容的长度对宽度进行调整，但整体效果还是不错吧！😃

> [查看效果](https://codepen.io/lio-zero/pen/zYwLObL)

## 更多资源

- [Typewriter effect](https://www.30secondsofcode.org/css/s/typewriter-effect) 通过 JS 获取文本的长度，并为其设置一个 CSS 变量，动态设置元素的宽度。这里使用 `1ch` 单位，相当于一个字形的长度。
- [typeit](https://github.com/alexmacarthur/typeit) 一个通用的 JavaScript 打字机效果。
- [Typewriter Animation That Handles Anything You Throw at It](https://css-tricks.com/typewriter-animation-that-handles-anything-you-throw-at-it/)
- [Typewriter Effect](https://css-tricks.com/snippets/css/typewriter-effect/)
