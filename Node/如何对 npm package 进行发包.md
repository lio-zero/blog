# 如何对 npm package 进行发包

在发布公共 package 之前，需要在 [npm 官网](https://www.npmjs.com/)进行注册一个账号。

随后，在本地项目执行命令 `npm login` 登录。

```bash
$ npm login
# 账号
# 密码
# 邮箱
# 一次性密码验证
```

最后，执行 `npm publish` 发包：

```bash
npm publish
```

> **注意**：`package.json` 不能将 `private` 设置为 `true`，他会将包标记为私有。

一旦发包成功，我们就可以像其他依赖包一样，通过 `npm i xxx` 安装我们发布的包，在项目上使用。

如果我们更新了该包，需要再次发包，可以使用 `npm version` 命令，控制该版本进行升级，注意需要遵循 [Semver 规范](https://github.com/semver/semver/blob/master/semver.md)。

```bash
# 增加一个修复版本号
$ npm version patch

# 增加一个小的版本号
$ npm version minor

# 将更新后的包发布到 npm 中
$ npm publish
```

在发布 npm 包时，我们一般都只发布构建后的资源，这时我们可以使用 `package.json` 的 `files` 字段。

```json
{
  "files": ["dist"]
}
```

它描述了在使用 `npm publish` 时推送到 npm 服务器的文件列表，支持目录和通配符，我们也可以在 `.gitignore` 或者 `.npmignore` 文件内排除不需要上传的文件。

但有一点需要注意，无论我们怎么设置，有些文件会始终被包含发包内，比如：

- `package.json`
- `README`
- `LICENSE / LICENCE`
- `package.json` 内 `main` 字段的文件

有一些文件则会始终被排除在发包内，比如：

- `.git`
- `.DS_Store`
- etc

最后，当你发包成功后，可以在 [npm devtool](https://npm.devtool.tech/zx) 查看各项数据。

![npm devtool](https://upload-images.jianshu.io/upload_images/18281896-6a0b37bec731ae98.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 更多资料

[A Comprehensive Guide To Creating and Publishing Your First NPM Package](https://eedris.hashnode.dev/a-comprehensive-guide-to-creating-and-publishing-your-first-npm-package)
