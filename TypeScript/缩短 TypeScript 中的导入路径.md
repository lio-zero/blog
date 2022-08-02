# 缩短 TypeScript 中的导入路径

在 TypeScript 中，我们通常使用相对路径导入特定文件。以下是从 `helper` 和 `services` 文件夹导入多个文件的示例：

```js
import { validator } from '../../../helpers/validator'
import { authService } from '../../../services/authService'
```

缺点是，当我们更改目录结构时，必须相应地更新这些导入。尽管 VS Code 等流行编辑器会自动更新路径，但这并不能确保该过程始终有效。

幸运的是，TypeScript 提供了使用绝对路径的能力。在 TypeScript 配置文件 `tsconfig.json` 中，我们可以在 `path` 属性下指示特定路径的别名。

例如，以下设置将在 `src` 文件夹中找到所有以 `@` 开头的导入：

```json
{
  "paths": {
    "@/*": ["src/*"]
  }
}
```

| 导入路径                 | 完全相同的绝对路径         |
| ------------------------ | -------------------------- |
| `@/helpers/validator`    | `src/helpers/validator`    |
| `@/services/authService` | `src/services/authService` |

我们的 `import` 可以缩短如下：

```js
import { validator } from '@/helpers/validator'
import { authService } from '@/services/authService'
```

例如，有些库遵循特定的目录结构模式，我们可以预先定义给定文件夹的路径：

```json
{
  "paths": {
    "@helpers/*": ["src/helpers/*"],
    "@models/*": ["src/models/*"],
    "@services/*": ["src/services/*"]
  }
}
```

然后，可以将属于这些文件夹的文件的导入缩短如下：

```js
import { validator } from '@helpers/validator'
import { userModel } from '@models/user'
import { authService } from '@services/authService'
```
