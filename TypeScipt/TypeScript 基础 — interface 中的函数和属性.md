# TypeScript 基础 — interface 中的函数和属性

在接口中定义方法有两种方法。

- 声明为类型为函数的属性

```ts
interface Logger {
  log: (message: string) => void
}
```

- 声明为正常函数

```ts
interface Logger {
  log(message: string): void
}
```

## 区别

如果将该方法声明为接口函数，则可以添加更多重载版本。

```ts
interface Logger {
  log(message: string): void
}

// 在其他地方
interface Logger {
  log(message: string, level: string): void
}
```

另一方面，将方法声明为属性可以防止复制具有不同类型的属性声明：

```ts
interface Logger {
  log: (message: string) => void
}

// 不起作用
interface Logger {
  log: (message: string, level: string) => void
}
```

`readonly` 修饰符仅对属性声明有效。

```ts
interface Person {
  firstName: string
  lastName: string

  readonly fullName: () => string

  // 不起作用
  // readonly fullName(): string
}
```

TypeScript 为实现接口方法的类生成不同的输出。

假设我们有一个 `ConsoleLogger` 类，它只在控制台窗口中记录消息。

对于第一种方法：

```ts
interface Logger {
  log: (message: string) => void
}

class ConsoleLogger implements Logger {
  log = (message: string) => {
    console.log(message)
  }
}

// 生成的 JavaScript 代码：
//
// class ConsoleLogger {
//  constructor() {
//    this.log = (message) => {
//      console.log(message)
//    }
//  }
// }
```

对于第二种方法：

```ts
interface Logger {
  log(message: string): void
}

class ConsoleLogger implements Logger {
  log(message: string) {
    console.log(message)
  }
}

// 生成的 JavaScript 代码：
//
// class ConsoleLogger {
//  log(message) {
//    console.log(message)
//  }
// }
```

查看生成的 JavaScript 代码，您将看到不同的输出。

第一种方法在构造函数中生成属性 `log`。这意味着每次创建类的新实例时都会创建 `log`。

而第二种方法生成 `log` 方法，并且它存在于类的所有实例中。`log` 方法也是类 `prototype` 的成员，因此如果需要，我们可以扩展该类以重写该方法：

```ts
class ConsoleLogger implements Logger {
  log(message: string) {
    console.log(message)
  }
}

class ConsoleLoggerWithColor extends ConsoleLogger {
  // 重写 log 方法
  log(message: string) {
    // 以白色和蓝色背景区域显示消息
    console.log('%c%s', 'color: white; background: blue', message)
  }
}
```
