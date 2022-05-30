# HTML autocomplete 属性

HTML `autocomplete` 属性为 `<input>` 字段提供了各种各样的选项。其作用是向浏览器指示值是否应在适当时自动填充。

`autocomplete` 允许浏览器预测对字段的输入。当用户在字段开始键入时，浏览器基于之前键入过的值，应该显示出在字段中填写的选项。

其最常见的两个常规值：

- `on` —— 默认。允许浏览器自动完成输入。 没有提供有关该字段中期望的数据类型的指导，因此浏览器可以使用自己的判断。
- `off` —— 浏览器不允许为此字段自动输入或选择一个值。文档或应用程序可能提供其自己的自动完成功能，或者出于安全方面的考虑，要求不要自动输入该字段的值。
- 完整的 `autocomplete` 值可查看 [values](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/autocomplete#values)。

> **注意**：许多现代浏览器在某些情况下故意忽略某些字段上的 `off` 值，以便让用户对自动填充字段有更多的控制。一个例子是使用密码管理器。

```html
<input autocomplete="on|off" />
```

下面将另一个常用的值 `new-password`，它将用于密码输入框。

## new-password

`new-password` —— 当要求用户创建新密码（例如，注册或重置密码输入框）时，可以使用该值。此值可确保浏览器不会意外填写现有密码，同时还允许浏览器建议一个安全密码：

```html
<form action="signup" method="post">
  <input type="text" autocomplete="username" />
  <input type="password" autocomplete="new-password" />
  <input type="submit" value="注册" />
  <button type="reset">重置</button>
</form>
```

此外，如果表单包含第二个密码字段，要求用户重新键入新创建的密码，则两个密码字段都应使用 `autocomplete="new-password"`。

以下是 Chrome 的密码管理器，你可以在**设置/自动填充/密码**下管理他们：

![new-password](https://upload-images.jianshu.io/upload_images/18281896-f41a2b2eb765766c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![建议一个安全密码](https://upload-images.jianshu.io/upload_images/18281896-646c3b3067cab408.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![密码管理器](https://upload-images.jianshu.io/upload_images/18281896-35f7ea4b24423251.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![管理](https://upload-images.jianshu.io/upload_images/18281896-a0ab823aa26b6e8f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 其他注意点

`autocomplete` 属性适用于下面的 `<input>` 类型控件：`text`、`search`、`url`、`tel`、`email`、`password`、`datepickers`、`range` 和 `color`。

**注意**：在某些浏览器中，您可能需要手动启用自动完成功能（在浏览器菜单的**参数设置**中进行设置）。

以下是 Can I Use 下 [autocomplete](https://caniuse.com/?search=autocomplete%20) 的支持情况

![autocomplete 的支持情况](https://upload-images.jianshu.io/upload_images/18281896-278c20c2ee90c614.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
