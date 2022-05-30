# 使用单选按钮切换 CSS 自定义属性

我们有一组单选按钮，分别切换不同的背景颜色，其主要结构如下：

```html
<form class="controls">
  <input type="radio" name="color" value="night-fade" checked />
  <input type="radio" name="color" value="warm-flame" />
  <input type="radio" name="color" value="spring-warmth" />
</form>
```

以往，我们可以使用 JavaScript 来检测用户何时与单选按钮交互并相应地附加一个类。

```js
const bgColor = document.querySelector('body')
const controls = document.querySelector('.controls')

let currentClass = 'night-fade'

const onChange = (e) => {
  if (!e.target.value || !e.target.checked) return

  if (bgColor.classList.contains(currentClass)) {
    bgColor.classList.replace(currentClass, e.target.value)
  } else {
    bgColor.classList.add(e.target.value)
  }

  currentClass = e.target.value
}

controls.addEventListener('change', onChange)
```

然后，我们为每个类添加 CSS `background-image` 来切换背景：

```css
body {
  background-image: linear-gradient(to top, #a18cd1 0%, #fbc2eb 100%);
}

body.warm-flame {
  background-image: linear-gradient(
    45deg,
    #ff9a9e 0%,
    #fad0c4 99%,
    #fad0c4 100%
  );
}

body.spring-warmth {
  background-image: linear-gradient(to top, #fad0c4 0%, #ffd1ff 100%);
}
```

效果如下：

![b7e2e755-9df1-45c0-bc20-c73fb479359a (1).gif](https://upload-images.jianshu.io/upload_images/18281896-a1c4a4f636b58534.gif?imageMogr2/auto-orient/strip)

然而，现在我们可以有更好的选择：**CSS 自定义属性（也称变量）**。它可以使你的 CSS、JS 看起来更加简洁、方便，我们同样使用它来完成上面的效果。

## 自定义属性

我们可以全局范围内，也就是 `:root` 内为接下来需要用到背景色值，都分配一个自定义属性：

```css
:root {
  --bg-color-night-fade: linear-gradient(to top, #a18cd1 0%, #fbc2eb 100%);
  --bg-color-warm-flame: linear-gradient(
    45deg,
    #ff9a9e 0%,
    #fad0c4 99%,
    #fad0c4 100%
  );
  --bg-color-spring-warmth: linear-gradient(to top, #fad0c4 0%, #ffd1ff 100%);
}
```

接下来，我们在本地范围内，也就是需要应用背景色的元素内（这里是 `body`）定义了一个新的 `--bg-color` 变量指定一个初始背景色。它将用于后面在单选按钮改变时，通过 JS 动态改变 `--bg-color` 内的值，使得 `background-image` 和 `--bg-color` 同步更新。

我们使用第一个 `--bg-color-night-fade` 变量作为第一个单选按钮选项相对应的初始值：

```css
body {
  --bg-color: var(--bg-color-night-fade);
  background-image: var(--bg-color);
}
```

最后，监听表单下单选按钮的 `change`，在 `onChange` 事件处理程序中，我们使用与所选 `radio` 相对应的自定义属性更新 `--bg-color` 的值。

```js
const bgColor = document.querySelector('body')
const controls = document.querySelector('.controls')

const onChange = (e) => {
  if (!e.target.value || !e.target.checked) return

  bgColor.style.setProperty('--bg-color', `var(--bg-color-${e.target.value})`)
}

controls.addEventListener('change', onChange)
```

你可以在这 👉[查看效果](https://codepen.io/lio-zero/pen/bGqqRqe)。
