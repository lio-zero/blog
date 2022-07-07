# Mongoose 中的枚举

Mongoose `String` 和 `Number` 类型有一个 `enum` 验证器。`enum` 验证器是一个数组，它将检查给定的值是否是数组中的项。如果该值不在数组中，当您尝试 save() 时，Mongoose 将抛出 `ValidationError`。

```js
const testSchema = new mongoose.Schema({
  status: {
    type: String,
    enum: ['valid', 'invalid']
  }
})

const Test = mongoose.model('Test', testSchema)

await Test.create({ name: 'not a status' }) // 引发验证错误
await Test.create({ name: 'valid' }) // ✅
```

## TypeScript 枚举

您还可以使用 [TypeScript 枚举](https://www.typescriptlang.org/docs/handbook/enums.html)。在运行时，TypeScript 枚举只是 POJO，其中对象的值就是枚举值。当您设置 `enum` 为一个对象时，Mongoose 将在对象上运行 `Object.values()` 来获取所需的值。

```js
enum Status {
  Valid,
  Invalid
}

const testSchema = new mongoose.Schema({
  rating: {
    type: Number,
    enum: Status
  }
})

const Test = mongoose.model('Test', testSchema)

await Test.create({ name: 'invalid' }) // 引发验证错误
await Test.create({ name: 'Valid' }) // ✅
```
