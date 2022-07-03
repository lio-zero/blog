# Node.js 递归获取文件夹中的所有文件

我需要递归地获取文件夹中的所有文件。

我发现这样做的最好方法是安装 [glob](https://www.npmjs.com/package/glob) 库：

```bash
pnpm i glob
```

我想查找所有 `README.md` 文件包含夹 `/post` 包含的所有文件 ，每个文件都在自己的目录结构中，可能在多个子文件夹下：
我想查找所有包含在 `/post` 文件夹内的所以 `README.md` 文件，每个文件都位于其自己的目录结构中，可能位于多个子文件夹下：

- `/post/HTML/README.md`
- `/post/CSS/README.md`
- `/post/other/test/README.md`

```js
const glob = require('glob')

const root_folder = './post'

glob(root_folder + '/**/README.md', (err, files) => {
  if (err) {
    console.log('Error', err)
  } else {
    console.log(files)
  }
})
/*
[
  './post/HTML/README.md',
  './post/CSS/README.md',
  './post/other/test/README.md'
]
*/
```
