# 对于在新标签页中打开的链接，始终使用 "noopener" 或 "noreferrer"

为了在新选项卡中打开链接，我们使用 `target="_blank"` 属性。然而，如果你稍不留意，它可能会导致一些问题。

首先，新打开的选项卡使用与打开的选项卡相同的进程。因此，它会降低页面速度。更重要的是，新选项卡能够通过 `window.opener` 对象访问 `opener` 页面的 `window` 对象。假设新选项卡执行以下代码：

```js
window.opener.location = 'https://example.com'
```

正如代码所暗示的，它将原始选项卡重定向到一个假网站。如果假网站的用户界面与真网站相同，会发生什么？由于用户已经打开了它，他们可能没有意识到他们正在查看的网站不是真实的。

添加 `rel="noopener"` 修复了这些问题。它不仅修复了 `noopener` 所做的问题，而且还阻止了 `Referer` 头被发送到新选项卡。

```html
<!-- 不要这样做 -->
<a target="_blank">...</a>

<!-- 像这样 -->
<a target="_blank" rel="noopener">...</a>

<!-- 或者 -->
<a target="_blank" rel="noreferrer">...</a>
<a target="_blank" rel="noopener noreferrer">...</a>
```

一些现代浏览器，如 Chrome 88+，如果缺少 `noopener` 行为，会自动添加它。但是，仍然建议手动添加 `rel="noopener"` 或 `rel="noreferrer"`，以避免旧浏览器中的安全性和性能问题。
