# 使用 Nodemailer 发送电子邮件

[Nodemailer](https://nodemailer.com/about/) 是一个对 Node.js 零依赖的单一模块，专为发送电子邮件而设计。其主要特点包括（但不限于）：

- 平台独立性
- 安全性，特别是使用 TLS/STARTTLS 和 DKIM 电子邮件身份验证的电子邮件传送
- Unicode 支持
- HTML 内容和嵌入的图像附件
- 除了 SMTP 支持之外，还有不同的传输方法。
- etc

## 安装

```bash
npm i nodemailer
```

> 版本要求 Node >= 6.0，如果想要与 `async/await` 一起使用，Node 需要大于 v8.0.0。

导入：

```js
const nodemailer = require('nodemailer')
```

## 发送电子邮件

要使用 Nodemailer 发送消息，有三个主要步骤。

- 创建 Nodemailer 传输器
- 设置 Nodemailer [消息选项](https://nodemailer.com/message/)
- 使用 `sendMail()` 发送消息

### 创建 Nodemailer 传输器

SMTP 是最常见的传输器。但是还有其他可用选项的列表：

- 内置传输
  - `sendmail` — 用于简单消息的常规 `sendmail` 命令。类似于 PHP 中的 `mail()` 函数。
  - `SES` — 是一个围绕 AWS SDK 的 Nodemailer 包装器，通过使用 [Amazon SES](https://aws.amazon.com/ses/) 发送电子邮件来处理大量电子邮件。
  - `stream` — 用于测试目的的缓冲区，用于返回消息。
- 对外传输。简而言之，您可以创建自己的传输方式。

> 详细查阅 [transports](https://nodemailer.com/transports/)。

使用 SMTP，一切都非常简单。设置主机、端口、身份验证详细信息和方法，仅此而已。

```js
// 使用默认 SMTP 传输创建可重用的传输对象
let transporter = nodemailer.createTransport({
  host: 'smtp.163.com', // 发送端邮箱类型
  port: 465, // 端口号
  secureConnection: true, // 使用 SSL
  auth: {
    user: 'xxxxx', // 发送方的邮箱地址（自己的）
    pass: 'xxxxx' // mtp 验证码
  }
})
```

如果需要，您还可以添加 `debug` 和 `logger` 调试选项。

在此阶段验证 SMTP 连接是否正确也很有用：添加验证（回调）调用以测试连接和身份验证。

```js
transporter.verify((error, success) => {
  if (error) {
    console.log(error)
  } else {
    console.log('服务器已准备好接收我们的消息')
  }
})
```

### 设置 Nodemailer 消息选项

此时，我们应该指定发件人、邮件收件人以及邮件的内容。

记住，Unicode 是受支持的，因此您也可以包含表情符号！

要发送 HTML 格式的文本，不需要额外的属性，只需将 HTML 正文放入带有 `html` 属性的消息中。对于高级模板，您可以添加附件和嵌入图像。

我们先来看一下这个简单消息选项的例子：

```js
let mailOptions = await transporter.sendMail({
  from: {
    name: '帅气逼人',
    address: 'xxxx'
  }, // 发件人邮箱地址
  to: mail, // 收件人邮箱地址，可以同时发送多个。例如：'user1@example.com, user2@example.com'
  subject: '使用 Nodemailer 发送电子邮件', // 邮件标题
  text: '验证码为：' + code // 邮件内容，code 为发送的验证码信息，这里的内容可以自定义
  html: '<b>嘿! </b><br> 这是我使用 Nodemailer 发送的第一条消息🎉👏'
})
```

`text` 和 `html` 您可以任意选择一个，如果两者同时存在，`html` 优先级比 `text` 高。

#### Nodemailer 中的附件

您可以使用以下主要属性向 Nodemailer 中的邮件添加不同类型的数据：

- `filename` — 附加文件的名称。在这里，您也可以使用 Unicode。
- `content` — 附件的正文。它可以是字符串、缓冲区或流。
- `path` — 文件的路径，用于流式传输，而不是将其包含在消息中。对于大型附件来说，这是一个不错的选择。
- `href` — 附件 URL。还支持数据 URI。

```js
attachments: [
  {
    // 使用 utf-8 字符串作为附件
    filename: 'text.txt',
    content: 'hello world!'
  }
]
```

以上使用 `attachments` 属性来添加附件，其中每个对象代表一个附件。

还有一些可选属性，允许您添加特定的内容类型或内联图像。

`contentType` — 如果不设置它，它将从 `filename` 属性中推断出来。

```js
// 字符串附件
{
  filename: 'notes.txt',
  content: 'new important notes',
  contentType: 'text/plain' // 可选，将从文件名中检测到
}
```

`cid` — HTML 消息中的内联图像。请注意，CID 值应该是唯一的。

```js
// 文件流附件
{
  filename: 'matrix neo.gif',
  path: __dirname + '/assets/neo.gif',
  cid: 'neo@example.com' // 应该尽可能独特
}
```

`encoding` — 可以添加到字符串类型的内容中。它将根据您设置的编码值（`base64`、二进制等）将内容编码为缓冲区类型。

```js
// 二进制缓冲区附件
{
  filename: 'image.png',
  content: Buffer.from('iVBORw0KGgoAAAANSUhEUgAAABAAAAAQAQMAAAAlPW0iAAAABlBMVEUAAAD///+l2Z/dAAAAM0lEQVR4nGP4/5/h/1+G/58ZDrAz3D/McH8yw83NDDeNGe4Ug9C9zwz3gVLMDA/A6P9/AFGGFyjOXZtQAAAAAElFTkSuQmCC', 'base64')
}
```

### 使用 `sendMail()` 发送消息

一旦我们创建了一个传输器并配置了一条消息，我们就可以使用 `sendMail()` 方法发送它：

```js
transport.sendMail(mailOptions, (error, info) => {
  if (error) return console.log(error)
  console.log('Message sent: %s', info.messageId)
})
```

> 这里先讲这么多，后面会写一些例子进行补充。另外，Nodemailer 支持 [Ethereal Email](https://ethereal.email/) 提供的测试账户。创建一个 Ethereal 帐户并使用为您生成的用户名和密码。

## 更多资料

- [mailtrap](https://mailtrap.io/blog/) 众多关于电子邮件文章
- [官方示例](https://github.com/nodemailer/nodemailer/blob/master/examples/full.js)
- [nodemailer-amqp-example](https://github.com/nodemailer/nodemailer-amqp-example) 将 Nodemailer 与 RabbitMQ 一起使用的示例。
