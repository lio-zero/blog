# 防止 npm 安装不支持的 Node.js 版本

确保设置项目的使用特定的 Node.js 版本，使开发人员在 `git clone` 或 `git pull` 您的项目时，可以正常运行项目。

我们可以通过在 `package.json` 中设置 `engines` 属性来[指定版本范围](https://docs.npmjs.com/files/package.json#engines)。

```json
{
  "engines": {
    "node": ">=15.0.0"
  }
}
```

许多项目定义了 `engine` 属性，但没有强制执行所需的 Node.js 版本。在不支持 Node.js 版本的项目中运行 `npm install` 时，将显示以下警告（`EBADENGINE`）。

```bash
$ npm install

# npm WARN EBADENGINE Unsupported engine {
# npm WARN EBADENGINE   package: 'expamle@1.0.0',
# npm WARN EBADENGINE   required: { node: '>=15.0.0' },
# npm WARN EBADENGINE   current: { node: 'v14.16.0', npm: '7.6.3' }
# npm WARN EBADENGINE }
```

这就是 `npm` 在这个场景中所做的一切。它会显示警告，但不会失败并阻止用户继续操作。

## 如何防止不支持 Node.js 版本的 `npm install`

操作方法很简单，我们可以将一个本地 `npm` 配置文件（`.npmrc`）添加到模块/项目根目录中，[并显式启用严格的 Node.js 引擎处理](https://docs.npmjs.com/cli/v7/using-npm/config#engine-strict)。

```bash
# .npmrc
engine-strict=true
```

如果项目包含定义 `engine-strict=true` 的 `.npmrc`，其 Node.js 未满足版本要求，则无法运行 `npm install`。

> **注意**：`EBADENGINE` 将从 **WARN** 变为 **ERR**，安装过程将失败，状态码为 `1`。

```bash
$ npm install

# npm ERR! code EBADENGINE
# npm ERR! engine Unsupported engine
# npm ERR! engine Not compatible with your version of node/npm: expamle@1.0.0
# npm ERR! notsup Not compatible with your version of node/npm: expamle@1.0.0
# npm ERR! notsup Required: {"node":">=15.0.0"}
# npm ERR! notsup Actual:   {"npm":"7.6.3","node":"v14.16.0"}

# npm ERR! A complete log of this run can be found in:
# npm ERR!     /Users/node_cache/.npm/_logs/2021-08-09T15_15_48_050Z-debug.log
```

在这方面，[pnpm](https://pnpm.io/package_json#engines) 与 npm 保持一致。

## Yarn 呢?

[Yarn](https://yarnpkg.com/) 不需要额外的配置文件，默认情况下严格处理 `engines` 属性。这似乎是处理 Node.js 版本的正确方法。

```bash
$ yarn install

# yarn install v1.22.19
# info No lockfile found.
# [1/5] 🔍  Validating package.json...
# error engine-test@1.0.0: The engine "node" is incompatible with this module. Expected version ">=15.0.0". Got "14.16.0"
# error Found incompatible module.
# info Visit https://yarnpkg.com/en/docs/cli/install for documentation about this command.
```

这就是防止开发人员使用不受支持的 Node.js 版本的项目所需要的一切！
