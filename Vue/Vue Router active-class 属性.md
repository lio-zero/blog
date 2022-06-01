# Vue Router active-class 属性

[`active-class`](https://router.vuejs.org/zh/api/#active-class) 是 vue-router 模块的 `router-link` 组件的属性，当 `router-link` 标签被点击时将会应用这个样式。

有两种使用方式：

- 直接在路由 Router 构造函数中配置 `linkActiveClass` 属性

```js
export default new Router({
  linkActiveClass: 'active'
})
```

使用这种方式，会在每个 `router-link` 标签上引用这个样式（当被点击后），如果您只想在某部分 `router-link` 上使用，则可以选择第二种。

- 单独在 `router-link` 标签上使用 `active-class` 属性

```html
<router-link to="/about" active-class="active">about</router-link>
```

最后，为您选中的链接设置样式：

```css
a.router-link-exact-active {
  color: plum;
}

.active {
  font-size: 16px;
  font-weight: bold;
}
```
