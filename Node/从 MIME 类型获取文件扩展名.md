# 从文件扩展名中获取 MIME 类型

我正在通过表单发送文件，并且在使用了带有 `multipart/form-data` 的表单后，服务器端的文件对象位于 `req.files` 中。

其中提供了一些信息，比如路径、名称、大小、类型等：

```js
{
  logo: File {
    size: 121920,
    path: '/path/to/upload_b9e85b7cf989482a1760d82b77fd555a',
    name: '大头像.png',
    type: 'image/png',
    hash: null,
    lastModifiedDate: 2022-06-07T20:30:12.150Z,
    //...
  }
}
```

请注意，临时文件路径没有扩展名。

如果您使用服务器端的名称，则没有问题。但是我想更改它并使用我自己的命名约定，所以我只需要文件扩展名。

有两种获取方式：`path` 内置模块和 `mime-types` 包。

使用内置 `path` 模块：

```js
const path = require('path')
path.extname(req.files.logo.name) // .png
```

它不需要任何第三方库。

或者，您可以使用 [mime-types](https://www.npmjs.com/package/mime-types) 包并查看 [MIME 类型](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Basics_of_HTTP/MIME_Types)：

```js
const mime = require('mime-types')

mime.extension('text/plain') // txt
mime.extension('image/png') // png
```
