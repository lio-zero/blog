# Mongoose 时间戳（timestamps）

Mongoose `Schema` 有一个 [`timestamps` 选项](https://mongoosejs.com/docs/guide.html#timestamps)，告诉 Mongoose 自动管理文档上的 `createdAt` 和 `updatedAt` 属性。例如，下面介绍如何在 `User` 模型上启用时间戳。

```js
const userSchema = mongoose.Schema(
  {
    name: String
  },
  {
    timestamps: true
  }
)

const User = mongoose.model('User', userSchema)
const doc = await User.create({ name: 'O.O' })

doc.createdAt // 2021-08-20T22:36:59.414Z
doc.updatedAt // 2021-08-20T22:36:59.414Z

doc.createdAt instanceof Date // true
```

启用时间戳时，Mongoose 会将 `createdAt` 和 `updatedAt` 属性添加到模型中。默认情况下，`createdAt` 和 `updatedAt` 的类型为 `Date`。更新文档时，Mongoose 会自动增加 `updatedAt`。

```js
doc.name = 'D.O'
await doc.save()

doc.createdAt // 2021-08-20T22:36:59.414Z
doc.updatedAt // 2021-08-20T22:37:09.071Z
```

特定的 mongoose 模型[写入操作](https://mongoosejs.com/docs/api.html#query_Query-setOptions)允许您跳过 `timestamps`，前提是在 `Schema` 中设置了时间戳。为此，必须将 `timestamps` 设置为 `false`，并且该操作不会更新时间。

```js
const userSchema = mongoose.Schema(
  {
    name: String
  },
  {
    timestamps: true
  }
)

const User = mongoose.model('User', userSchema)

const doc = await User.findOneAndUpdate(
  {
    email: 'O.O'
  },
  {
    email: 'D.O'
  },
  {
    new: true,
    upsert: true,
    timestamps: false
  }
)
```

如果希望仅阻止其中一个更新，我们应该创建具有键值对的对象，而不是将 `timestamps` 设置为 `false` 作为值。根据需求，我们只需要根据需求将 `createdAt` 或 `updatedAt` 设置为 `true` 或 `false`。

```js
const userSchema = mongoose.Schema(
  {
    name: String
  },
  {
    timestamps: true
  }
)

const User = mongoose.model('User', userSchema)

const doc = await User.findOneAndUpdate(
  {
    name: 'O.O'
  },
  {
    name: 'D.O'
  },
  {
    new: true,
    upsert: true,
    timestamps: {
      createdAt: false,
      updatedAt: true
    }
  }
)
```

## 备用属性名

默认情况下，Mongoose 使用 `createdAt` 和 `updatedAt` 作为时间戳的属性名。但你可以让 Mongoose 使用任何你喜欢的属性名。例如，如果您更喜欢 `snake_case` 属性名，可以让 Mongoose 使用 `created_at` 和 `updated_at` 替代：

```js
const userSchema = mongoose.Schema(
  {
    name: String
  },
  {
    timestamps: {
      createdAt: 'created_at',
      updatedAt: 'updated_at'
    }
  }
)
const User = mongoose.model('User', userSchema)

const doc = await User.create({
  name: 'O.O'
})
doc.updated_at // 2021-08-20T22:40:06.667Z
```

## 使用 UNIX 时间戳

虽然日期类型通常已经足够了，但您也可以让 Mongoose 将时间戳存储为自 1970 年 1 月 1 日以来的秒数。Mongoose `Schema` 支持 `timestamps.currentTime` 选项，该选项允许您传递用于获取当前时间的自定义函数。

```js
const userSchema = mongoose.Schema(
  {
    name: String
  },
  {
    // 让 Mongoose 使用 UNIX 时间（自1970年1月1日起的秒数）
    timestamps: {
      currentTime: () => Math.floor(Date.now() / 1000)
    }
  }
)
```
