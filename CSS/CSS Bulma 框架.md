# CSS Bulma 框架

[Bulma](https://bulma.io/) 是一个免费的、开源的框架，采用了移动优先的响应式布局，它提供了可随时使用的前端组件，您可以轻松地将这些组件组合起来构建响应性强的 web 界面。

Bluma 可以作为 Bootstrap 的替代框架，这类框架还有 [Skeleton](http://getskeleton.com/)、[Pure](https://purecss.io/)、[Bootflat](http://bootflat.github.io/)、[Mueller](https://muellergridsystem.com/)。

Bluma 是纯 CSS 的框架，你只需要将已经给定的类添加到你的标签中，就能实现漂亮的效果。

下面我们将来介绍它。

## 安装

### 使用 NPM

```bash
npm install bulma
```

安装后，导入 CSS 文件：

```css
@import 'bulma/css/bulma.css';
```

### 使用 CDN

使用 [jsDelivr](https://www.jsdelivr.com/package/npm/bulma) 导入 CSS 文件

```js
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bulma@0.9.2/css/bulma.min.css">
```

### 本地

您可以从 GitHub 获得最新的 [Bulma](https://github.com/jgthms/bulma/blob/master/css/bulma.css) 版本，下载样式表文件到本地

## 屏幕尺寸

Bulma 是一个手机优先的框架，提供五个宽度断点，具有良好的自适应特性，可以随心所欲为不同设备设置不同样式。

```
         768         1024                1216         1408
'     '     '     '     '     '     '     '     '     '     '     '
<---------^------------^------------------^-------------^------------->
  mobile      tablet         desktop         widescreen      fullhd
```

## 网格体系

Bulma 的网格体系基于 Flex 布局，使用 `columns` 指定容器，使用 `column` 指定项目。

```html
<div class="columns">
  <div class="column"></div>
  <div class="column"></div>
  <div class="column"></div>
  <div class="column"></div>
  <div class="column"></div>
</div>
```

## 修饰符

以下类定义**不同颜色**。

```css
.is-primary .is-link .is-info .is-success .is-warning .is-danger;
```

以下类定义**大小**。

```css
.is-small .is-normal .is-medium .is-large;
```

以下类定义**状态**。

```scss
.is-hovered
.is-outlined
.is-loading
.is-focused
.is-active
.is-static
```

完整的修饰类清单请看[官方文档](https://bulma.io/documentation/elements/button/)。

## 排版

以下类修改**字体大小**。

| `.is-size-1` | 3rem    |
| ------------ | ------- |
| `.is-size-2` | 2.5rem  |
| `.is-size-3` | 2rem    |
| `.is-size-4` | 1.5rem  |
| `.is-size-5` | 1.25rem |
| `.is-size-6` | 1rem    |
| `.is-size-7` | 0.75rem |

可以为不同设备指定不同的文字大小。

| `is-size-1-mobile`        | 手机             |
| ------------------------- | ---------------- |
| `is-size-1-tablet`        | 平板             |
| `is-size-1-touch`         | 手机和平板       |
| `is-size-1-desktop`       | 桌面、宽屏和高清 |
| `is-size-1-widescreen`    | 宽屏和高清       |
| `size-1 is-size-1-fullhd` | 高清             |

以下类**对齐**文本

| `.has-text-centered`  | 使文本**成为中心** |
| --------------------- | ------------------ |
| `.has-text-justified` | 使文本**合理**     |
| `.has-text-left`.     | 使文本与**左**对齐 |
| `.has-text-right`     | 使文本向**右**对齐 |

以下类**转换**文本

| `.is-capitalized` | 将每个单词**的第一个字符**转换为**大写** |
| ----------------- | ---------------------------------------- |
| `.is-lowercase`   | 将**所有**字符转换为**小写**             |
| `.is-uppercase`   | 将**所有**字符转换为**大写**             |

## 组件

Bulma 内置了 `Breadcrumb`、`Card`、`Menu` 等十种组件，使用超级简单、方便，你可以在这 👉 [components](https://bulma.io/documentation/components/) 查看这些组件。

以下以 Card 卡片为例：

```html
<div class="card">
  <div class="card-content">
    <div class="content">
      Lorem ipsum leo risus, porta ac consectetur ac, vestibulum at eros.
    </div>
  </div>
  <div class="card-image">
    <figure class="image is-4by3">
      <img
        src="https://bulma.io/images/placeholders/1280x960.png"
        alt="Placeholder image"
      />
    </figure>
  </div>
  <footer class="card-footer">
    <a href="#" class="card-footer-item">Save</a>
    <a href="#" class="card-footer-item">Edit</a>
    <a href="#" class="card-footer-item">Delete</a>
  </footer>
</div>
```

以上例子中，卡片内分为三部分：`card-content` 文本内容，`card-image` 图片容器，`card-footer` 脚部列表。

## WYSIWYG 内容

> WYSIWYG：所见即所得是一种系统。它使得用户在视图中所看到文档与该文档的最终产品具有相同的样式，也允许用户在视图中直接编辑文本、图形、或文档中的其他元素。

```html
<div class="content">
  <!-- start WYSIWYG contents -->
  <h1>Heading</h1>
  <p>Paragraph</p>

  <ul>
    <li>Item 1</li>
    <li>Item 2</li>
  </ul>
  <!-- end WYSIWYG contents -->
</div>
```

要为通常生成的 WYSIWYG 内容提供默认样式，请使用 `.content` 类。你可以在[Content](https://bulma.io/documentation/elements/content/)查看关于这类的内容。

## 定制 Bulma

要想自定义 Bulma，您需要：

- 安装 Bulma
- 有效的 Sass 设置
- 创建自己的 `.scss` 和 `.sass` 文件

你可以通过 [node-sass](https://bulma.io/documentation/customize/with-node-sass)、[Sass CLI](https://bulma.io/documentation/customize/with-sass-cli)、[webpack](https://bulma.io/documentation/customize/with-webpack) 任何一种方法来实现，官网给出了详细的步骤，一步到位。下面我们简单的介绍下如何更改自定义样式。

先导入 Bulma [**初始变量**](https://bulma.io/documentation/customize/variables/)，以如下为例：

```scss
@import '../node_modules/bulma/bulma.sass'; // 该文件需要先引用

// 设置您需要更改颜色的变量
$purple: #8a4d76;
$pink: #fa7c91;
$purple-color-1: $purple; // 派生变量
```

上面代码中，预设的 `purple`、`pink` 和 `purple-color-1` 变量将被替换。
