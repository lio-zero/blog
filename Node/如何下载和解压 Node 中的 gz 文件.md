# 如何下载和解压 Node 中的 gz 文件

假如我想从网络上下载一个 gizp 文件，你要怎么处理这种文件格式？下面我们使用 Node 的一些库来解决这个问题。

- [**got**](https://github.com/sindresorhus/got#readme)：适用于 Node.js 的人性化且功能强大的 HTTP 请求库。
- [**Node-gzip**](https://www.npmjs.com/package/node-gzip)：只需使用 `promise` 在 Node.js 中使用 `gzip` 和 `ungzip`。

以下代码段使用 `got` 发出 HTTP 请求，并使用 `Node-gzip` 来获取 gzip sitemap，并将其转换为字符串。

```js
import got from 'got'
import pkg from 'node-gzip'
const { ungzip } = pkg

const SITEMAP_URL =
  'https://developer.mozilla.org/sitemaps/en-US/sitemap.xml.gz'

async function main() {
  // 获取文件
  const { body } = await got(SITEMAP_URL, {
    responseType: 'buffer'
  })

  // 解压缓冲的 gzip 站点地图
  const sitemap = (await ungzip(body)).toString()

  console.log(sitemap)
}

main()
```
