# Mongoose Schema 和 SchemaTypes

在 Mongoose 中，[`Schema`](https://mongoosejs.com/docs/guide.html) 是模型的配置对象。 `Schema` 不允许您从 MongoDB 读写，这就是模型的用途。

它可以：

- 定义保存在 MongoDB 中的文档可以具有哪些属性
- 定义自定义[验证](https://mongoosejs.com/docs/validation.html)
- 声明 [virtuals](https://mongoosejs.com/docs/guide.html#virtuals)
- 声明 [getter 和 setter](https://mongoosejs.com/docs/tutorials/getters-setters.html)
- 定义[静态](https://mongoosejs.com/docs/guide.html#statics)和[方法](https://mongoosejs.com/docs/guide.html#methods)

## Schema 路径和转换

[`Schema` 类构造函数](https://mongoosejs.com/docs/api/schema.html#schema_Schema)的第一个参数是一个 `definition` 对象。此对象定义 Schema 具有的路径。例如，下面的 `userSchema` 有一个 `name` 路径和一个 `age` 路径。

```js
const userSchema = new mongoose.Schema({
  name: String,
  age: Number
})

userSchema.path('name') // SchemaString { ... }
userSchema.path('age') // SchemaNumber { ... }
```

要在 Mongoose 中创建模型，可以调用 [`mongoose.model()`](https://mongoosejs.com/docs/api/mongoose.html#mongoose_Mongoose-model) 方法，并将 `Schema` 作为第二个参数。例如，下面示例中的 `UserModel` 将具有 `name` 和 `age` 属性，并将删除 `userSchema` 中未定义的任何属性。

```js
const userSchema = new mongoose.Schema({
  name: String,
  age: Number
})

const UserModel = mongoose.model('User', userSchema)

const doc = new UserModel({
  name: 'O.O',
  age: 18,
  hobby: 'programming'
})

console.log(doc.name) // 'O.O'
console.log(doc.age) // 59

// Mongoose 去掉了 hobby，因为它不在 schema 中
console.log(doc.hobby) // undefined
```

此外，Mongoose 将强制转换文档以匹配给定的 `Schema` 类型。这意味着您可以安全地将不受信任的数据传递给 Mongoose，并相信数据将与您的 `Schema` 匹配。

```js
const UserModel = mongoose.model('User', userSchema)

const doc = new UserModel({
  name: 'O.O',
  age: '18' // Mongoose 会将其转换为数字
})

console.log(doc.age) // 18
await doc.save()

// Mongoose 将 20 从字符串转换为数字，即使在更新中也是如此
await UserModel.updateOne({}, { $set: { age: '20' } })
```

## 验证

除了强制转换值，Mongoose 还允许您在 `Schema` 中定义验证。例如，假设您希望确保您的用户有一个 `name`。您可以在 `Schema` 中设置 `name` 属性 `required`，如下所示。

```js
const userSchema = new mongoose.Schema({
  // 将 name 设为 required，也就是该字段是必填项
  name: { type: String, required: true },
  age: Number
})
const UserModel = mongoose.model('User', userSchema)

const doc = new UserModel({ age: 30 })

const err = await doc.save().catch((err) => err)
console.log(err.message) // Path name is required.
```

## 选项

`Schema` 构造函数接受 2 个参数：`definition` 和 `options`。您可以在 [Mongoose 文档中找到 `Schema` 选项的完整列表](https://mongoosejs.com/docs/guide.html#options)。

例如，[`typeKey` 选项](https://mongoosejs.com/docs/guide.html#typeKey)允许您配置 Mongoose 查找哪个键，以确定您是否正在定义嵌套路径。假设要定义名为 `type` 的嵌套键：

```js
const schema = new mongoose.Schema({
  nested: {
    type: String
  }
})

schema.path('nested') // SchemaString { ... }
schema.path('nested.type') // undefined
```

此用例有[多种解决方法](https://mongoosejs.com/docs/faq.html#type-key)。一种是设置 `typeKey` 选项，如下所示。

```js
// 让 Mongoose 查找 $type 而不是 type
const options = { typeKey: '$type' }
const schema = new mongoose.Schema(
  {
    nested: {
      type: String
    },
    otherProperty: {
      $type: String
    }
  },
  options
)

schema.path('nested.type') // SchemaString { ... }
schema.path('otherProperty') // SchemaString { ... }
```

## SchemaType

在 Mongoose 中，[SchemaType](https://mongoosejs.com/docs/schematypes.html) 是 Schema 中单个路径的配置对象。SchemaType 说明路径应该是什么类型，如何验证该路径，路径的默认值是什么，以及其他特定于 Mongoose 的配置选项。

```js
const schema = new mongoose.Schema({ name: String, age: Number })

schema.path('name') instanceof mongoose.SchemaType // true
schema.path('age') instanceof mongoose.SchemaType // true
```

`SchemaType` 类只是一个基类。有几个类继承自 `SchemaType`，代表不同的核心 Mongoose 类型：

- `mongoose.Schema.Types.String`
- `mongoose.Schema.Types.Number`
- `mongoose.Schema.Types.Date`
- `mongoose.Schema.Types.Buffer`
- `mongoose.Schema.Types.Boolean`
- `mongoose.Schema.Types.Mixed`
- `mongoose.Schema.Types.ObjectId`（或等效的 `mongoose.ObjectId`）
- `mongoose.Schema.Types.Array`
- `mongoose.Schema.Types.Decimal128`
- `mongoose.Schema.Types.Map`

例如：

```js
const schema = new mongoose.Schema({ name: String, age: Number })

schema.path('name') instanceof mongoose.SchemaType // true
schema.path('name') instanceof mongoose.Schema.Types.String // true

schema.path('age') instanceof mongoose.SchemaType // true
schema.path('age') instanceof mongoose.Schema.Types.Number // true
```

您通常不必直接使用 `SchemaType` 实例。您可以在 `Schema` 定义中声明验证器和默认值。例如，下面的示例将默认 `age` 设置为 25，并添加一个验证器，以确保 `age` 至少为 21。

```js
const schema = new mongoose.Schema({
  age: {
    type: Number,
    default: 25,
    validate: (v) => v >= 21
  }
})
```

以上是您通常在 Mongoose 中声明默认值和验证器的方式。但是在创建模式之后，没有什么可以阻止您将它们添加到 `age` SchemaType 中。

```js
// 等价的
const schema = new mongoose.Schema({ age: Number })

schema.path('age').default(25)
schema.path('age').validate((v) => v >= 21)
```

后一种语法与前一种语法相同，但并不常用。直接使用 `SchemaType` 实例最常见的情况是使用[嵌入式鉴别器](https://thecodebarbarian.com/mongoose-4.12-single-embedded-discriminators.html)。
