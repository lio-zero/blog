# 什么是 Doctype？

任何 HTML 文档都必须在第一行以 doctype（Document Type Declaration，文件类型声明）开头，它告诉浏览器页面中使用的 HTML 版本。

此 doctype 声明（不区分大小写）：

```html
<!DOCTYPE html>
```

告诉浏览器这是一个 HTML5 文档。

## 浏览器渲染模式

通过此声明，浏览器可以在标准模式下渲染文档。

没有它，浏览器会以怪癖模式（Quirks Mode）渲染页面。

如果您从未听说过怪癖模式，那么您必须知道浏览器引入这种渲染模式是为了使以 "旧样式" 编写的页面与使用的新功能和标准兼容。如果没有它，随着浏览器和 HTML 的发展，旧页面会破坏它们的外观，而 Web 平台在这方面一直非常具有保护性。

浏览器基本上默认为怪癖模式，除非它们识别出该页面是为标准模式编写的。

还有一点需要注意，IE <= 10 用户要避免怪癖模式，可以使用：

```html
<meta http-equiv="X-UA-Compatible" content="IE=Edge" />
```

在页面 `<head>` 标签中，在加载任何脚本之前。

## 较旧的 HTML 版本

HTML 有一组奇怪的版本：

- HTML（1991）
- HTML 2.0（1995）
- HTML 3.2（1997）
- HTML 4.01（1999）
- XHTML（2000）
- HTML5（2014）

HTML 4.01 Strict 文档的文档类型为：

```html
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">
```

XHTML 类似：

```html
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
```

他们需要 DTD（文档类型定义），因为那些旧的 HTML 版本是基于 SGML，一种定义文档结构的格式。

XHTML 还要求 HTML 标签具有命名空间，如下所示：

```html
<html xmlns="http://www.w3.org/1999/xhtml"></html>
```

那些 doctype 声明总是要求您将 DTD 声明保存在某个地方，因为几乎不可能记住。此外，对于严格模式或过渡模式（不太严格）有不同的 DTD。

XHTML 是一个 XML 词汇表，而 HTML4（及更低版本）是 SGML 应用程序。当前的 HTML，即 HTML5，在很大程度上受到 HTML4 的启发，但不是 SGML 应用程序，并且放弃了 XHTML 的许多严格规则。

HTML5 不是基于 SGML，而是基于它自己的标准，因此不需要 DTD，我们从这个非常简单的声明中受益：

```html
<!DOCTYPE html>
```
