# 隐藏 Microsoft Edge 的密码显示按钮

![image.png](https://upload-images.jianshu.io/upload_images/18281896-9c58577904f79002.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

如果您使用的是 Edge，需要隐藏 `input` 的 `password` 类型提供的**密码显示**按钮，可以使用下面的伪元素。

```css
::-ms-reveal {
  display: none;
}
```

> [新的 CSS 属性`input-security`正在使密码显示更容易](https://twitter.com/stefanjudis/status/1457281480556781568)。

https://www.stefanjudis.com/snippets/how-to-hide-microsoft-edges-password-reveal-button/

https://docs.microsoft.com/en-us/microsoft-edge/web-platform/password-reveal
