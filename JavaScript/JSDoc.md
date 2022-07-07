# JSDoc

[JSDoc](https://jsdoc.app/) 是许多代码库中使用的一种流行的内联文档方法。

编写 JSDoc 是为了增强代码的可读性，以及方便导出 API 文档。

## 为什么要使用 JSDoc？

JSDoc 是 DocBlock 的 JavaScript 实现，DocBlock 是一种用于各种编程语言（包括 PHP、Ruby 和 Python）的文档方法。

它提供了一种一致且可识别的文档方法。还有几个工具可以根据 [JSDoc](https://github.com/jsdoc/jsdoc) 注释自动生成文档。

## 将文档注释添加到您的代码

JSDoc 的目的是记录您的 JavaScript 应用程序或库的 API。假设您需要为模块、命名空间、类、方法和方法参数等内容添加注释。

最简单的方法是创建一个带有两个前导星号 ( `/**`) 的注释，以及对函数功能的描述。

```js
/**
 * 将两个数字相加
 */
function add(num1, num2) {
  return num1 + num2
}
```

添加描述很简单，只需在文档注释中键入您想要的描述即可。

特殊的 **JSDoc 标签**可用于提供更多信息。最常用的是 `@param`，它描述了一个函数参数，和 `@return`，它描述了返回的内容。

使用 JSDoc 标签来描述您的代码：

```js
/**
 * 将两个数字相加
 * @param  {Number} num1 要添加的第一个数字
 * @param  {Number} num2 要添加的第二个数字
 * @return {Number}      总数
 */
function add(num1, num2) {
  return num1 + num2
}
```

更多标签可用于添加更多信息。有关 JSDoc 的完整标签列表，请参阅 [Block Tags](https://jsdoc.app/index.html#block-tags)。

## 生成网站

添加代码注释后，您可以使用 JSDoc 工具从源文件生成 HTML 网站。

默认情况下，JSDoc 使用内置的**默认**模板将文档转换为 HTML。您可以编辑此模板以满足您自己的需求，或者创建一个全新的模板。

在命令行上运行文档生成器：

```bash
npx jsdoc index.js
```

此命令将在当前工作目录中创建以 `/out` 命名的目录。在该目录中，您将找到生成的 HTML 页面。

以上述的代码为例，生成的页面如下：

![JSDoc 生成文档](https://upload-images.jianshu.io/upload_images/18281896-1490cc4c5508fda3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 最后

我相信，很多开发人员都不喜欢写注释。

很多开发人员都追求**代码的自我描述性**，代码的目的非常明显，不需要文档。

对你来说显而易见的东西可能对阅读你代码的其他人来说并不明显。适当的添加一些注释，可以帮助他们更快、更容易地工作。

好的文档不仅仅描述发生了什么，还描述了为什么要这样做。一年后，当你去看一段时间没有接触过的代码时，你会忘记这些内容。

## 更多资料

[Document This](https://marketplace.visualstudio.com/items?itemName=oouo-diogo-perdigao.docthis) 是一个 VS Code 扩展，可以自动为 TypeScript 和 JavaScript 文件生成详细的 JSDoc 注释。

另一个不错的 JSDoc 注释 扩展 — [Add jsdoc comments](https://marketplace.visualstudio.com/items?itemName=stevencl.addDocComments)

![演示](https://github.com/Microsoft/vscode-comment/raw/master/images/addDocComments.gif)
