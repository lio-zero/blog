# 如何在 JavaScript 中遍历任意深度的对象

有时我们会发现我们需要遍历一个对象，并在某个任意深度对其执行一些操作。虽然这看起来很困难，但我们可以利用递归、可变性和对象引用来快速完成任务。

## 示例：深层占位符替换

假设，我们有以下数据：

```js
const user = {
  name: 'IU',
  hobby: ['eat', 'sleep', '[placeholder]'],
  friend: {
    name: 'UI',
    hobby: ['eat', 'sleep', 'laugh', '[placeholder]'],
    friend: {
      name: 'Lay',
      hobby: ['dance', 'vocal', '[placeholder]']
    }
  }
}
```

我们需要做的是，将 `[placeholder]` 替换为我们提供的内容。

### 分析

- 创建一个以对象作为参数的函数。
- 对于该对象中的每个键，如果该键是 `string`，则执行占位符替换
- 如果键是 `object`，则将该键返回给初始化函数进行递归！

### JavaScript 实现如下

```js
const replacePlaceholder = (obj) => {
  for (let key in obj) {
    if (typeof obj[key] === 'string')
      obj[key] = obj[key].replace('[placeholder]', 'rap')
    if (typeof obj[key] === 'object') replacePlaceholder(obj[key])
  }
}

replacePlaceholder(user)
console.log(JSON.stringify(user, null, 2))
/*
{
  "name": "IU",
  "hobby": [
    "eat",
    "sleep",
    "rap"
  ],
  "friend": {
    "name": "UI",
    "hobby": [
      "eat",
      "sleep",
      "laugh",
      "rap"
    ],
    "friend": {
      "name": "Lay",
      "hobby": [
        "dance",
        "vocal",
        "rap"
      ]
    }
  }
}
*/
```

可以看到，我们实现了占位符替换。**值得注意的是**，我们没有从 `replacePlaceholder` 函数返回任何东西，而是沿状态树往下执行改变了输入对象！

如果这不是您期望的行为，您可以考虑实现深拷贝，首先拷贝对象，然后对拷贝进行突变。
