# 使用 closest 和 matches 方法来检测元素是否存在某选择器

假如我们有一下 HTML 结构：

```html
<article class="post cat-5">
  <header>
    <h2>Title</h2>
  </header>
  <div class="post-content">
    <p class="summary">summary</p>
  </div>
</article>
```

我们可以使用 `closest` 和 `matches` 方法来检测元素是否存在某选择器。

- [`closest()`](https://developer.mozilla.org/en-US/docs/Web/API/Element/closest) 方法遍历元素及其父元素（指向根文档），如果当前元素与给定选择器匹配，则返回当前元素，否则返回与给定选择器匹配的最近祖先元素，否则返回 `null`。

- [`matches()`](https://developer.mozilla.org/en-US/docs/Web/API/Element/matches) 方法检查 DOM 元素是否与给定的选择器匹配。

```js
const summary = document.querySelector('.summary')
const post = summary.closest('.post')

console.log(post.matches('article')) // true
console.log(post.matches('.post')) // true
console.log(post.matches('.post.cat-5')) // true
console.log(post.matches('div.post')) // false
```
