# flexible.js 源码解读

flexible.js 是移动端的一种使用 rem 适配方案。

源码相对简单，需要知道的前置知识：

- 使用 `rem` 作为单位，因为 `rem` 是一个相对单位，它相对于根元素的 `font-size`，页面元素的大小计算都与根元素进行换算。（默认浏览器的大小 16px）
- `window.devicePixelRatio` 它将返回当前显示设备的物理像素分辨率与 CSS 像素分辨率之比，也就是设备像素比（DPR）。通过设备屏幕的 DPR，自动设置最合适当前设备的高清缩放。

[源码地址](https://github.com/amfe/lib-flexible/blob/2.0/index.js)。

这里需要解读的代码量相对较少，我直接在代码内进行注释：

```js
;(function flexible(window, document) {
  // 获取根元素
  var docEl = document.documentElement
  // 获取设备像素比
  var dpr = window.devicePixelRatio || 1

  // 调整 body 字体大小，也就是子元素的默认字体大小都继承自 body
  function setBodyFontSize() {
    if (document.body) {
      document.body.style.fontSize = 12 * dpr + 'px'
    } else {
      document.addEventListener('DOMContentLoaded', setBodyFontSize)
    }
  }

  setBodyFontSize()

  // set 1rem = viewWidth / 10
  // 也就是 1rem 等于视口/屏幕可视区域宽度的 1/10，将宽度分割为十等分
  // 例如：iphone 6 的屏幕宽度为 375px，因此 375 / 10 = 37.5px = 1rem
  function setRemUnit() {
    var rem = docEl.clientWidth / 10
    docEl.style.fontSize = rem + 'px'
  }

  setRemUnit()

  // 页面调整时重置 rem
  window.addEventListener('resize', setRemUnit)
  // pageshow 事件在每次加载页面时触发（包括后退/前进按钮操作，同时也会在 onload 事件触发后初始化页面时触发）
  window.addEventListener('pageshow', function (e) {
    // persisted 属性表示网页是否是来自缓存
    if (e.persisted) {
      setRemUnit()
    }
  })

  // 检测是否支持 0.5px，解决 1px 在高清屏多像素问题
  if (dpr >= 2) {
    var fakeBody = document.createElement('body')
    var testElement = document.createElement('div')
    testElement.style.border = '.5px solid transparent'
    fakeBody.appendChild(testElement)
    docEl.appendChild(fakeBody)
    if (testElement.offsetHeight === 1) {
      docEl.classList.add('hairlines')
    }
    docEl.removeChild(fakeBody)
  }
})(window, document)
```

相信整体看下来，您已经清楚了 flexible.js 的工作方式。

另外，如果使用到了 rem 适配方案，推荐阅读[手机端页面自适应解决方案—rem 布局进阶版（附源码示例）](https://www.jianshu.com/p/985d26b40199?u_atoken=6f66415f-631b-4cd1-bd8c-5731dd1a859c&u_asession=01fvkFYMKnBoEy2E8G8BFbIn85xGPgM31gvh_adnUM1w8c3hYlNQpqbjZ-wQfizXZpX0KNBwm7Lovlpxjd_P_q4JsKWYrT3W_NKPr8w6oU7K8zLRMGNR49ySM19OxyvpgyMKWrbBzYAhXhkL4v5_cjQmBkFo3NEHBv0PZUm6pbxQU&u_asig=05_XdN3PuBKh5kzXoHuxuBPybAeiXsANjpMmnnryRPi86-6eF8eCqMc1OqbStFqG6qPSBFYaxdJuhr1owQkP3-BqBhVMH0EMxjsAVh8Gb3q-x0b_RDAlipzbVghLgnzys8_3xW5SWZCLuPu3m8ofI4lT30AnlbXisUC4pcfY7LfMX9JS7q8ZD7Xtz2Ly-b0kmuyAKRFSVJkkdwVUnyHAIJzRH3sFyUHmRo6b8qI5NgOwvy7cq83-wFqfkvvwL4d3Ewdf9JIAkyKervFWgmMgV8j-3h9VXwMyh6PgyDIVSG1W-6p5luVNYr4n8BYf4nuWlzL6GwjCufiNkuwxPu3TW8JRaFEjRJqyCA1ysRwX2uVxpTVqEilRH0ZED7dJnEnXMjmWspDxyAEEo4kbsryBKb9Q&u_aref=gsYwJ%2BPjOV6iwFB%2BmX%2BRbnMI4gg%3D)。

## 最后

推荐阅读一下有关 CSS 单位的知识：[CSS 单位及其需要注意的地方](https://github.com/lio-zero/blog/blob/master/CSS/CSS%20%E5%8D%95%E4%BD%8D%E5%8F%8A%E5%85%B6%E9%9C%80%E8%A6%81%E6%B3%A8%E6%84%8F%E7%9A%84%E5%9C%B0%E6%96%B9.md)。

如今，视口单位已经得到了很好的支持，且能很好的解决横屏问题，比较推荐使用。

以下在推荐几篇移动端适配相关的文章：

- [前端基础知识概述 -- 移动端开发的屏幕、图像、字体与布局的兼容适配](https://juejin.cn/post/6844903935568789517)
- [移动端适配总结](https://juejin.cn/post/6844903734347055118#heading-14)
- [面试官：你了解过移动端适配吗？](https://juejin.cn/post/6844903631993454600)
- [记一次移动端使用 rem 的兼容性问题](https://juejin.cn/post/6844903798436003854)
