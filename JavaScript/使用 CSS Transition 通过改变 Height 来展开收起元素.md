# 使用 CSS Transition 通过改变 Height 来展开收起元素

一个常见的开发需要，我们希望折叠元素的一部分，直到需要它为止。一些常见的框架（如 `Bootstrap` 和 `jQuery`）提供了转换效果。然而，CSS `Transition` 在转换高度方面给予了我们很大的灵活性。因此，您不必在项目中加入其他框架。

下面我们看看，如何使用 CSS `Transition` 设置 `height` 属性的动画效果以及其存在的问题、解决方案。

你可以在 👉[查看效果](https://codepen.io/lio-zero/pen/JjWLzqP)

## 过渡高度

我们要实现的操作是，当点击**查看更多**按钮将增加元素的高度，以显示文章的所有内容，再次点击时，它将收回到原本的样子。

我们将为此创建一个 CSS 类。单击**查看更多**按钮时，将使用 JavaScript 将此类添加到 `<article>` 元素上。

首先，我们将向 HTML 文件添加一个 `<article>` 元素，并为其添加一些内容。

```html
<article id="article">
  <h3>使用 CSS Transition 通过改变 Height 来展开收起元素</h3>
  <p>
    Lorem ipsum, dolor sit amet consectetur adipisicing elit. Illo perspiciatis
    tempora iure accusamus rerum. Fuga porro unde, laboriosam soluta accusantium
    numquam quos adipisci commodi velit, expedita officia cum excepturi
    molestias.
  </p>

  <p>
    Lorem ipsum dolor sit amet consectetur adipisicing elit. Ab doloribus optio,
    eaque harum blanditiis totam voluptatibus amet quibusdam veritatis animi
    ipsum eveniet modi aspernatur, vel repellat est commodi consequatur unde! A
    obcaecati soluta inventore, numquam impedit quaerat magnam incidunt sit
    cupiditate sequi cum. Exercitationem commodi reiciendis culpa iste optio
    aliquam incidunt at, ab consectetur quae est sapiente dignissimos, sit
    deleniti voluptatibus animi repudiandae. Itaque nemo laborum dolore numquam
    repudiandae mollitia quis. Placeat quis architecto eligendi distinctio quas
    perferendis officia voluptatem illo, nisi ullam voluptatum odio eveniet non
    eum vero vel dolorum deleniti adipisci culpa. Reprehenderit cum ut
    voluptates reiciendis iusto.
  </p>

  <p>
    Lorem ipsum dolor sit amet consectetur adipisicing elit. Ab doloribus optio,
    eaque harum blanditiis totam voluptatibus amet quibusdam veritatis animi
    ipsum eveniet modi aspernatur, vel repellat est commodi consequatur unde! A
    obcaecati soluta inventore, numquam impedit quaerat magnam incidunt sit
    cupiditate sequi cum. Exercitationem commodi reiciendis culpa iste optio
    aliquam incidunt at, ab consectetur quae est sapiente dignissimos, sit
    deleniti voluptatibus animi repudiandae. Itaque nemo laborum dolore numquam
    repudiandae mollitia quis. Placeat quis architecto eligendi distinctio quas
    perferendis officia voluptatem illo, nisi ullam voluptatum odio eveniet non
    eum vero vel dolorum deleniti adipisci culpa. Reprehenderit cum ut
    voluptates reiciendis iusto.
  </p>

  <p>
    Lorem ipsum dolor sit amet consectetur adipisicing elit. Ab doloribus optio,
    eaque harum blanditiis totam voluptatibus amet quibusdam veritatis animi
    ipsum eveniet modi aspernatur, vel repellat est commodi consequatur unde! A
    obcaecati soluta inventore, numquam impedit quaerat magnam incidunt sit
    cupiditate sequi cum. Exercitationem commodi reiciendis culpa iste optio
    aliquam incidunt at, ab consectetur quae est sapiente dignissimos, sit
    deleniti voluptatibus animi repudiandae. Itaque nemo laborum dolore numquam
    repudiandae mollitia quis. Placeat quis architecto eligendi distinctio quas
    perferendis officia voluptatem illo, nisi ullam voluptatum odio eveniet non
    eum vero vel dolorum deleniti adipisci culpa. Reprehenderit cum ut
    voluptates reiciendis iusto.
  </p>
</article>
<button id="seeMoreBtn">查看更多</button>
```

CSS 样式如下：

```css
article {
  max-width: 800px;
  height: 300px;
  overflow-y: hidden;
}

/* 单击按钮时添加此类 */
article.extended {
  height: 628px;
}

button {
  padding: 0.6rem;
}
```

JavaScript 如下：

```js
const seeMore = document.getElementById('seeMoreBtn')
const article = document.getElementById('article')

seeMore.addEventListener('click', () => {
  article.classList.toggle('extended')

  const extended = article.classList.contains('extended')
  if (extended) {
    seeMore.innerHTML = '收起内容'
  } else {
    seeMore.innerHTML = '查看更多'
  }
})
```

向 `article` 添加 CSS `transition` 过渡属性，使其点击按钮时内容可以丝滑的上下滑动。

```css
article {
  max-width: 800px;
  height: 300px;
  overflow-y: hidden;
  transition: height 0.4s linear;
}
```

现在将它应用到你的文章，你可以看到它可以上下丝滑的滑动着，很简单、方便，但此方法存在的限制。下面我们来看看。

## 限制

这个限制体现在是否知其高度，以上的例子我们清楚的知道文章的高度，它工作的效果非常好，但当我们在处理动态内容时，我们不知道元素的高度，其高度也可能随着屏幕大小或其他方式进行变化。

其实解决方式也很简单，对于动态内容，元素的高度应设置为 `auto`。这样，任何增加或减少的元素高度都将自适应。但也会出现另一个问题：**当元素的高度设置为 `auto` 时，CSS `transition` 将不起作用**。

好消息是，有一种方法可以解决此问题，而不必求助于更多的 JavaScript。

## 解决方案

解决方法是转换 `max-height` 属性而不是 `height` 属性。首先，我们必须估计我们的元素能达到的最大高度。然后，当元素展开时，我们将该元素设置为 `max-height` 大于我们的估计值。

我们将 `height` 属性改为 `max-height`：

```css
article {
  max-width: 800px;
  max-height: 300px;
  overflow-y: hidden;

  /* 增加过渡时间以适应高度 */
  transition: max-height 0.7s linear;
}

article.expanded {
  max-height: 1500px;
}
```

这样动画的工作原理，我们仍然得到我们想要的效果。过渡时间可能需要根据您需要的效果进行调整。
