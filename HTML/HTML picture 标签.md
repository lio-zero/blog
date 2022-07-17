# HTML picture 标签

HTML 为我们提供了 [`picture` 标签](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/picture)，它与 `img` 标签的 `srcset` 属性非常相似，并且差异非常细微。

当你想要完全改变文件，而不是仅仅服务于一个小版本的文件时，你就可以使用 `picture`。或者提供不同的图像格式。

我发现的最佳用例是在提供 [WebP 图像](https://github.com/lio-zero/blog/blob/main/WTF/WebP%20%E5%9B%BE%E5%83%8F%E6%A0%BC%E5%BC%8F.md)时，它虽然已经在主流浏览器中得到广泛支持，但你知道的，IE 是个例外。

在 `picture` 标签中，您指定了一个图像列表，并将按顺序使用这些图像，因此以下示例中，支持 WebP 的浏览器将使用第一个图像，如果不支持则回退到 JPG：

```html
<picture>
  <source type="image/webp" srcset="image.webp" />
  <img src="image.jpg" alt="An image" />
</picture>
```

`source` 标签定义了图像的一种（或多种）格式。如果浏览器非常旧并且不支持 `picture` 标签，`img` 标签是备用的。

在 `picture` 内的 `source` 标签中，您可以添加 `media` 属性以设置媒体查询。

下面的示例类似于上述 `srcset` 示例：

```html
<picture>
  <source media="(min-width: 500w)" srcset="avatar-500.png" sizes="100vw" />
  <source media="(min-width: 800w)" srcset="avatar-800.png" sizes="100vw" />
  <source media="(min-width: 1000w)" srcset="avatar-1000.png" sizes="800px" />
  <source media="(min-width: 1400w)" srcset="avatar-1400.png" sizes="800px" />
  <img src="avatar.png" alt="一张头像" />
</picture>
```

但这不是它的用例，因为正如你所看到的，它要详细得多。

## 更多资料

[使用 srcset 的响应式图像](https://github.com/lio-zero/blog/blob/main/HTML/%E4%BD%BF%E7%94%A8%20srcset%20%E7%9A%84%E5%93%8D%E5%BA%94%E5%BC%8F%E5%9B%BE%E5%83%8F.md)
