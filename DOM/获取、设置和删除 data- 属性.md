# 获取、设置和删除 data-* 属性

## 获取 `data-*` 属性的值

获取`ele`元素的`data-message`属性

```js
const message = ele.getAttribute('data-message')

// Or
const message = ele.dataset.message
```

## 设置 `data-*` 属性的值

```js
ele.setAttribute('data-message', 'Hello World')

// Or
ele.dataset.message = 'Hello World'
```

## 删除 `data-*` 属性

```js
ele.removeAttribute('data-message')

// Or
delete ele.dataset.message
```

> **注意**，调用 `delete ele.dataset` 不会删除所有 `data` 属性。
