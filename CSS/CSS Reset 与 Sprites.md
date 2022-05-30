# CSS Reset 与 Sprites

总结起来就两段话：

- Reset —— 重置浏览器默认属性
- Sprites —— 由多个小图片组成的图像。目的是为了提高性能，减少服务器对图片的请求数

## 为什么要初始化 CSS 样式

因为浏览器的兼容问题，不同浏览器对有些标签的默认值是不同的，如果没对 CSS 初始化往往会出现浏览器之间的页面显示差异。

当然，初始化样式会对 SEO 有一定的影响，但鱼和熊掌不可兼得，但力求影响最小的情况下初始化。

`*` 是最简单的初始化方法（不建议）：

```css
* {
  padding: 0;
  margin: 0;
}
```

但最好的做法是使用 CSS Reset 或 Normalize.css。

## CSS Reset 和 Normalize.css 有什么区别？

- [Normalize.css](https://necolas.github.io/normalize.css/) 是一种 CSS Reset 的替代方案，一种现代的 HTML5 替代 CSS 重置方法。
- **重置（Resetting）**： 重置意味着除去所有的浏览器默认样式。对于页面所有的元素，像 `margin`、`padding`、`font-size` 这些样式全部置成一样。你将必须重新定义各种元素的样式。
- **标准化（Normalizing）**： 标准化没有去掉所有的默认样式，而是保留了有用的一部分，同时还纠正了一些常见错误。

### 目的

- 与许多 CSS Reset 不同，保留有用的默认值。
- 规范各种元素的样式。
- 修复浏览器自身的 bug 并保证各浏览器的一致性
- 通过细微的修改来提高可用性。
- 使用详细注释说明代码的作用。

### 区别

- Normalize.css 保护了有价值的默认值
- Normalize.css 修复了浏览器的 bug
- Normalize.css 不会让你的调试工具变的杂乱
- Normalize.css 是模块化的
- Normalize.css 拥有详细的文档

### 我们该如何去做选择呢？

当需要实现非常个性化的网页设计时，我会选择重置的方式，因为我要写很多自定义的样式以满足设计需求，这时候就不再需要标准化的默认样式了。

## 什么是精灵图（CSS Sprites），其优缺点，以及如何实现？

精灵图，也称雪碧图。精灵图是把多张小图片整合到一张图片中。它被运用在众多使用了很多小图标的网站上

### 实现方法

- 使用生成器将多张图片打包成一张精灵图，并为其生成合适的 CSS。
- 每张图片都有相应的 CSS 类，该类定义了 `background-image`、`background-position` 和 `background-size` 属性。
- 使用图片时，将相应的类添加到你的元素中。

### 优点

- 减少 HTTP 请求数（一张精灵图只需要一个请求），极大地提高页面加载速度。但是对于 HTTP2 而言，加载多张图片不再是问题。
- 提前加载资源，防止在需要时才在开始下载引发的问题，比如只出现在 `:hover` 伪类中的图片，不会出现闪烁。
- 增加图片信息重复度，提高压缩比，减少图片大小。
- 更换风格方便，只需在一张或几张图片上修改颜色或样式即可实现。

### 缺点

- 图片合并麻烦。
- 维护麻烦，修改一个图片可能需要从新布局整个图片，样式。

### 扩展

- 在 HTTP/1.1 下，每个 TCP 连接最多允许一个请求
- 使用 HTTP/1.1，现代的浏览器可以打开多个并行连接（2 到 8 之间），但是受到限制
- 使用 HTTP/2，浏览器和服务器之间的所有请求都在单个 TCP 连接上多路复用
- 这意味着可以减少打开和关闭多个连接的成本，从而可以更好地使用 TCP 连接并限制客户端和服务器之间的延迟影响。这样就可以在同一 TCP 连接上并行加载数十个图像
- 尽管 HTTP/2 比 HTTP/1.1 改进了 50％，但在大多数情况下，sprites 的加载速度仍然比单个图像快。
- 一种较新的技术是使用图标字体，它具有基于矢量的附加优点，因此在高分辨率（视网膜）屏幕上看起来更好。一些不错的图标字体是 [Awesome 字体](http://fortawesome.github.com/Font-Awesome/)和 [Foundation 图标](http://www.zurb.com/playground/foundation-icons)

### 替代方案

- **Sprites 是栅格图像**，替代的方案通常是与其相关的，有 [SVG 图标](https://simurai.com/post/20251013889/svg-stacks)，[字体图标](https://css-tricks.com/flat-icons-icon-fonts/)，[字符编码](https://copypastecharacter.com/)等等。
- Sprites 就是为了减少服务器对图片的请求数，提高网站性能的一种手段。我们可以从解决图像请求的问题入手，也就是对图像进行懒加载 [lazyload](https://appelsiini.net/projects/lazyload/)，还有渐进式[图像加载占位符](https://jmperezperez.com/medium-image-progressive-loading-placeholder/)。

### 提高效率实用工具

- [CSS Sprites Generato](https://www.toptal.com/developers/css/sprite-generator) 在线生成器
- [SpriteCow](http://www.spritecow.com/)
- [SpriteMe](https://spriteme.org/)
- [Grunticon](https://github.com/filamentgroup/grunticon) 几年没维护了
