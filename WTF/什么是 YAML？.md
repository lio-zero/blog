# 什么是 YAML？

> YAML 是 "YAML Ain't a Markup Language"（YAML 不是一种标记语言）的递归缩写。在开发的这种语言时，YAML 的意思其实是："Yet Another Markup Language"（仍是一种标记语言），但为了强调这种语言以数据做为中心，而不是以标记语言为重点，而用反向缩略语重命名。

[官网](https://yaml.org/)原文是这样解释的：

> YAML is a human-friendly data serialization language for all programming languages.
>
> 翻译：YAML 是一种适用于所有编程语言的人性化数据序列化语言。

您可以将数据序列化视为将结构化数据集移动到某些编程语言中可用对象的过程。

## 数据序列化语言的快速比较

JSON 和 XML 是您可能熟悉的另外两种序列化语言。YAML 类似于 JSON 和 XML，但自称更具可读性。让我们看一个例子。

假设我们想在一个 `posts` 数组中表示两篇博客文章。每个帖子都将包含以下属性：

- `title`（字符串）
- `tags`（字符串数组）
- `body`（字符串，格式为 markdown）

### XML

使用占位符内容时，此帖子数组在使用 XML 时可能与以下内容类似：

```xml
<posts>
  <post>
    <title>xxx</title>
    <tags>
      <tag>xxx</tag>
      <tag>xxx</tag>
      <tag>xxx</tag>
    </tags>
    <body>
      Lorem ipsum dolor sit amet consectetur adipisicing elit. Nulla quidem at, suscipit quo ullam
      quaerat vitae perspiciatis aliquid delectus, quas mollitia, unde consequuntur commodi
      cupiditate officia natus? Quis, fugiat consectetur.
    </body>
  </post>
  <post>
    <title>xxx</title>
    <tags>
      <tag>xxx</tag>
      <tag>xxx</tag>
      <tag>xxx</tag>
    </tags>
    <body>
      Lorem ipsum dolor sit amet consectetur adipisicing elit. Nulla quidem at, suscipit quo ullam
      quaerat vitae perspiciatis aliquid delectus, quas mollitia, unde consequuntur commodi
      cupiditate officia natus? Quis, fugiat consectetur.
    </body>
  </post>
</posts>
```

结构明显且僵硬。它具有 HTML 的类似，但它很冗长。例如，注意某个标签比标签本身需要更多字符。

### JSON

这是使用 JSON 构建相同内容时的样子：

```json
{
  "posts": [
    {
      "title": "xxx",
      "tags": ["xxx", "xxx", "xxx"],
      "body": "Lorem ipsum dolor sit amet consectetur adipisicing elit. Nulla quidem at, suscipit quo ullam quaerat vitae perspiciatis aliquid delectus, quas mollitia, unde consequuntur commodi cupiditate officia natus? Quis, fugiat consectetur."
    },
    {
      "title": "xxx",
      "tags": ["xxx", "xxx", "xxx"],
      "body": "Lorem ipsum dolor sit amet consectetur adipisicing elit. Nulla quidem at, suscipit quo ullam quaerat vitae perspiciatis aliquid delectus, quas mollitia, unde consequuntur commodi cupiditate officia natus? Quis, fugiat consectetur."
    }
  ]
}
```

在某些情况下，这可能更具可读性，`tags` 是一个简洁而明显的数组，而在其他情况下则更加困难，例如 `body`，因为 JSON 不适合多行字符串。

### YAML

现在让我们看一下 YAML 中的相同数据：

```yaml
posts:
  - title: xxx
    tags:
      - xxx
      - xxx
      - xxx
    body: |-
      Lorem ipsum dolor sit amet consectetur adipisicing elit. Nulla quidem at, suscipit quo ullam
      quaerat vitae perspiciatis aliquid delectus, quas mollitia, unde consequuntur commodi
      cupiditate officia natus? Quis, fugiat consectetur.
  - title: xxx
    tags:
      - xxx
      - xxx
      - xxx
    body: |-
      Lorem ipsum dolor sit amet consectetur adipisicing elit. Nulla quidem at, suscipit quo ullam
      quaerat vitae perspiciatis aliquid delectus, quas mollitia, unde consequuntur commodi
      cupiditate officia natus? Quis, fugiat consectetur.
```

可以看到，非常简洁。

注意 YAML 的以下特征：

- 我们并不（总是）需要引号（在某些情况下，引号是必要的）。
- 包装文本很简单（`body` 可读性强）。
- 我们只需要几个有趣的字符来表示我们正在处理的对象的类型，比如一个连字符（`-`）意味着我们正在处理一个数组。

## YAML 的优劣

YAML 已经在很多地方流行起来，因为它与 JSON 和 XML 等替代方案相比可读性很强。和 Markdown 一样，它也意味着读写起来又快又容易。

但在处理深度嵌套的结构时，YAML 显的很麻烦。因为 YAML 依赖于结构的缩进，当嵌套多层时很容易丢失。

随着复杂性的增加，JSON 和 XML 等语言的僵化开始显得更受欢迎。

## 何时使用 YAML

正是出于这些原因，我更倾向于支持 YAML：

- 相对扁平的对象。
- 将手动编辑的文件（当数据结构要以编程方式操作时，我更倾向于使用 JSON）。

## JavaScript 相关工具

- [YAML Checker](https://yamlchecker.com/) 提供了一种快速简便的方法来验证 YAML。
- [yaml](https://github.com/eemeli/yaml) 用于 YAML 的 JavaScript 解析器和字符串生成器
- [js-yaml](https://github.com/nodeca/js-yaml) JavaScript YAML 解析器和转储器。

## 最后

各种编程语言的 YAML 用法可以查阅[官网](https://yaml.org/)，教程可以阅读阮一峰老师的 [YAML 语言教程](https://www.ruanyifeng.com/blog/2016/07/yaml.html)。
