# Mongoose Virtuals

[Mongoose virtuals](https://mongoosejs.com/docs/guide.html#virtuals) 是 [Mongoose Documents](https://mongoosejs.com/docs/documents.html) 上的计算属性。它们不存储在 MongoDB 中：每当您访问它时，都会计算一个 virtual 属性。

假设您有一个 `BlogPost` 模型，该模型存储 BlogPost 的原始 [markdown](https://www.markdownguide.org/) 内容。您可以创建一个虚拟 `html`，每当您访问 `html` 属性时，它会自动为您调用 [marked](http://npmjs.com/package/marked) 解析器。

```js
const marked = require('marked')

const blogPostSchema = new Schema({ content: String })

// _virtual_ 是一个 schema 属性，它不是存储在 MongoDB 中，而是根据 document 中的其他属性计算。
blogPostSchema.virtual('html').get(function () {
  // 在 getter 函数中，this 是 document，因此不要对 virtual getter 使用箭头函数！
  return marked(this.content)
})

const BlogPost = mongoose.model('BlogPost', blogPostSchema)
const doc = new BlogPost({ content: '# Hello' })
console.log(doc.html) // "<h1 id="hello">Hello</h1>"
```

**为什么要使用 `virtual` 而不是 [methods](https://mongoosejs.com/docs/guide.html#methods)呢？**

因为您可以配置 Mongoose 在将 Mongoose 文档转换为 JSON 时，包括使用 Express 的 `res.json()` 方法时，包含 virtuals。

```js
const app = require('express')()
const axios = require('axios')

// 无论何时调用 JSON.stringify() 或 res.json()，都要让 Mongoose 附加 virtuals
mongoose.set('toJSON', { virtuals: true })

app.get('*', function (req, res) {
  // Mongoose 将自动附加虚拟 html
  res.json(doc)
})

const server = app.listen(3000)

await axios.get('http://localhost:3000').then((res) => console.log(res.data.html)) // "<h1 id="hello">Hello</h1>"
```

**virtuals 也有缺点**，由于它不存储在 MongoDB 中，因此您无法在[查询](https://mongoosejs.com/docs/queries.html)中使用它们。
