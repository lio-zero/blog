# 使用 Express 上传文件

以下方法使用 AJAX 封装了上传文件的简单示例，将所选文件从 `fileEle` 元素发送到后端：

```js
const upload = function (fileEle, backendUrl) {
  return new Promise((resolve, reject) => {
    // 获取所选文件的列表
    const files = fileEle.files

    // 创建一个新的 FormData
    const formData = new FormData()

    // 在文件上循环
    [].forEach.call(files, function (file) {
      formData.append(fileEle.name, file, file.name)
    })
    // or => [...files].forEach(file => formData.append(fileEle.name, file, file.name))

    // 创建新的 AJAX 请求
    const req = new XMLHttpRequest()
    req.open('POST', backendUrl, true)

    // 处理事件
    req.onload = function () {
      if (req.status >= 200 && req.status < 400) {
        resolve(req.responseText)
      }
    }
    req.onerror = function () {
      reject()
    }

    // 发送它
    req.send(formData)
  })
}
```

## 用法

假设我们有一个允许用户选择多个文件的文件输入：

```html
<input type="file" id="upload" name="files" multiple />
```

> **注意**：这里我们需要提供一个 `name` 属性，后面我们在后端获取时需要用到。

我们可以在执行上传按钮的单击事件处理程序中使用以下代码：

```js
const fileEle = document.getElementById('upload')

fileEle.addEventListener('change', async () => {
  const res = await upload(fileEle, 'http://localhost:5000/upload')
  const data = JSON.parse(res)
  console.log(data)
  // ...
})
```

## 使用 Node Express 快速搭建一个服务

使用 Express 快速搭建一个服务，这里我们还使用文件上传库 [Formidable](https://www.npmjs.com/package/formidable)。

以下是使用 Formidable 获取上传文件的操作：

```js
const app = require('express')()
const formidable = require('formidable')
const port = 5000

app.post('/upload', (req, res) => {
  const form = new formidable.IncomingForm()
  // 多文件可以传入 options 对象，设置 multiples: true
  // 详见文档：https://www.npmjs.com/package/formidable

  // 解析 req 并上载所有相关文件
  form.parse(req, (err, fields, files) => {
    if (err != null) {
      console.log(err)
      return res.status(400).json({ message: err.message })
    }

    // files 对象包含已上载的所有文件。Formidable 将解析每个文件并为您上载到临时文件。
    const [firstFileName] = Object.keys(files)

    res.json({ filename: firstFileName })
  })
})

app.listen(port, () => {
  console.log(`Server is running on port ${port}`)
})
```

`/upload` 路由处理有 3 个步骤：

- 使用 `new formidable.IncomingForm()` 创建一个新表单。[`IncomingForm` 类](https://www.npmjs.com/package/formidable#api)是实现该功能都主要入口。
- 对 Express [request](http://expressjs.com/en/4x/api.html#req) 调用 `form.parse()`。这会告诉 Formidable 解析请求并将请求中的任何文件保存到服务器。
- 处理上传的文件。您可以将文件存储在本地，也可以将文件[七牛云](https://developer.qiniu.com/kodo/1289/nodejs)等服务。`
