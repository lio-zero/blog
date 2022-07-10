# Emmet 语法

[Emmet](https://docs.emmet.io/)（前身为 Zen Coding）是许多流行的文本编辑器的插件，可以极大地改善 HTML 和 CSS 工作流程，提高前端开发效率。

Emmet 在大部分编辑器都支持，如：Sublime Text、VS Code 等。使用时，按下面所接受的基本语法编写，按 **tab** 键或者回车就会自动生成对应的 HTML 标签。

官网也提供了一份超详细的[备忘单](https://docs.emmet.io/cheat-sheet/) 和 [PDF 文档](https://docs.emmet.io/cheatsheet-a5.pdf)。以下内容将以备忘单为参考，提供常用的基本语法。

## HTML 模板

从 HTML 模板开始，在空的 html 文件中输入 `!` 会触发 Emmet 的建议：

```html
!
```

它将生成一个 HTML5 基本模板。

## 后代节点：>

```css
nav>ul>li
```

效果如下：

```html
<nav>
  <ul>
    <li></li>
  </ul>
</nav>
```

## 兄弟节点：+

```css
section>p+p+p
```

效果如下：

```html
<section>
  <p></p>
  <p></p>
  <p></p>
</section>
```

## 上级节点：^

```css
section>header>h1^footer
```

效果如下：

```html
<section>
  <header>
    <h1></h1>
  </header>
  <footer></footer>
</section>
```

当嵌套过深，可以使用多個 `^^`

```css
.container>header>h1>span^^footer
```

效果如下：

```html
<div class="container">
  <header>
    <h1><span></span></h1>
  </header>
  <footer></footer>
</div>
```

## ID（#）和类（.）

```css
ul.menu>li.menu__item+li#id_item+li.menu__item#active
```

效果如下：

```html
<ul class="menu">
  <li class="menu__item"></li>
  <li id="id_item"></li>
  <li class="menu__item" id="active"></li>
</ul>
```

## 分组：()

```css
section>(header>nav>ul>li)+footer>p
```

效果如下：

```html
<section>
  <header>
    <nav>
      <ul>
        <li></li>
      </ul>
    </nav>
  </header>
  <footer>
    <p></p>
  </footer>
</section>
```

可以与 `*` 结构，生成重复结构

```css
ul>(li>a[href="#"]{item $})*3
```

效果如下：

```html
<ul>
  <li><a href="#">item 1</a></li>
  <li><a href="#">item 2</a></li>
  <li><a href="#">item 3</a></li>
</ul>
```

## 重复：*

```css
ul>li*3
```

效果如下：

```html
<ul>
  <li></li>
  <li></li>
  <li></li>
</ul>
```

## 编号：$

当 `$` 符号出现一次时，从 1 开始。当 `$$...` 出现多个时，从 0 开始。

```css
ul>li.item$*3
ul>li.item$$*3
ul>li.item$@-*3
ul>li.item$@3*5
```

效果如下：

```html
<ul>
  <li class="item1"></li>
  <li class="item2"></li>
  <li class="item3"></li>
</ul>
<ul>
  <li class="item01"></li>
  <li class="item02"></li>
  <li class="item03"></li>
</ul>
<ul>
  <li class="item3"></li>
  <li class="item2"></li>
  <li class="item1"></li>
</ul>
<ul>
  <li class="item3"></li>
  <li class="item4"></li>
  <li class="item5"></li>
</ul>
<ul>
  <li class="item3"></li>
  <li class="item4"></li>
  <li class="item5"></li>
  <li class="item6"></li>
  <li class="item7"></li>
</ul>
```

## 属性：[]

```css
input[type="text"]
div[data-attr="test"]
```

效果如下：

```html
<input type="text" />
<div data-attr="test"></div>
```

## 文本：{}

```css
p{一段文本}
```

效果如下：

```html
<p>一段文本</p>
```

## 隐式标签

隐式标签表示 Emmet 可以省略某些标签名。

```css
.container
em>.default-inline
ul>.default-list
table>.default-table-row>.default-table-column
select>.default-select
```

效果如下：

```html
<div class="container"></div>
<em><span class="text"></span></em>
<ul>
  <li class="list"></li>
</ul>
<table>
  <tr class="table-row">
    <td class="table-column"></td>
  </tr>
</table>
<select name="" id="">
  <option class="select"></option>
</select>
```
