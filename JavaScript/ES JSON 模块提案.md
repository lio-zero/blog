# ES JSON 模块提案

JSON 模块已经存在于 Chrome 91，它看起来就像一个 ES Modules 风格的导入，只是你在最后设置了类型。

```js
import configData from './config-data.json' assert { type: 'json' }
```

**好处**：一行代码获取 JSON 数据
