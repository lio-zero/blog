# TypeScript 文字联合类型与字符串枚举

在 TypeScript 中，有几种方法可以枚举类型的可能值。让我们考虑一个情况，我们的网站必须支持不同的主题，包括 `default`、`light` 和 `dark`。

它们可以通过以下方法之一定义：

- 使用文字字符串类型的并集

```ts
type Theme = 'DEFAULT' | 'LIGHT' | 'DARK'
```

- 使用枚举

```ts
enum Theme {
  DEFAULT,
  LIGHT,
  DARK
}
```

## 区别

TypeScript 不会为文字字符串类型的并集生成代码。因此，生成的代码将具有较小的大小。

以下是 TypeScript 编译 `Theme` 枚举时生成的代码：

```ts
var Theme
;(function (Theme) {
  Theme[(Theme['DEFAULT'] = 0)] = 'DEFAULT'
  Theme[(Theme['LIGHT'] = 1)] = 'LIGHT'
  Theme[(Theme['DARK'] = 2)] = 'DARK'
})(Theme || (Theme = {}))
```

在使用联合类型的情况下，创建新变量时仍必须键入完整字符串，例如：

```ts
type Theme = 'DEFAULT' | 'LIGHT' | 'DARK'
const theme: Theme = 'DEFAULT'
```

像 VS Code 这样的流行编辑器可以帮助我们快速地从可能的值列表中选择值，但是如果我们想将值重构为其他值，我们必须在所有位置手动更改该值。

另一方面，当您重构代码或开发库时，使用 `enum` 会带来一些显著的好处。让我们来看一个简单的用例，您的库提供以下功能来将网站切换到给定的主题：

```ts
const switchTheme = (theme: Theme) => {
  // ...
}

export { Theme, switchTheme }
```

用户现在可以将特定主题传递给 `switchTheme` 函数，而无需考虑其背后的真正值：

```ts
import { Theme, switchTheme } from 'your-lib'

switchTheme(Theme.DARK)
```

与 `switchTheme('DARK')` 相比，上面的调用是多么方便和安全。

## 很高兴知道

自 TypeScript 3.4 起，我们可以使用 `const` 断言。

```ts
const Theme = {
  DEFAULT: 'Default',
  LIGHT: 'Light',
  DARK: 'Dark'
} as const

type Theme = typeof Theme[keyof typeof Theme]

let darkTheme: Theme = Theme.DARK
let lightTheme: Theme = 'Light'
const invalidTheme: Theme = 'Blue' // type error
```

## 建议

如果未设置枚举的值，则默认情况下，这些值将设置为递增数。

因此，`Theme.DEFAULT`、`Theme.LIGHT` 和 `Theme.DARK` 的值分别为 0、1 和 2。调试更加困难：

```ts
console.log(Theme.DARK) // 2
```

即使我们为枚举值设置了数字，我们仍然可以为类型为枚举的变量设置无效值：

```ts
enum Theme {
  DEFAULT = 0,
  LIGHT = 1,
  DARK = 2
}

// TypeScript 不会抛出错误
const theme: Theme.DEFAULT = 3
```

由于这些原因，建议对枚举值使用字符串文字。`Theme` 枚举应如下所示：

```ts
enum Theme {
  DEFAULT = 'Default',
  LIGHT = 'Light',
  DARK = 'Dark'
}

console.log(Theme.DARK) // 'Dark'

// 不能将类型 Default 分配给类型 Theme.DEFAULT
let theme: Theme.DEFAULT = 'Default' // type error
```

## 更多资料

- [TypeScript 基础 — 枚举](https://github.com/lio-zero/blog/blob/master/TypeScipt/TypeScript%20%E5%9F%BA%E7%A1%80%20%E2%80%94%20%E6%9E%9A%E4%B8%BE.md)
- [TypeScript 基础 — 联合类型](https://github.com/lio-zero/blog/blob/master/TypeScipt/TypeScript%20%E5%9F%BA%E7%A1%80%20%E2%80%94%20%E8%81%94%E5%90%88%E7%B1%BB%E5%9E%8B.md)
