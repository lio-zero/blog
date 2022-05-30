# 将 JSON 数据下载为文件

两种方法实现：

- 通过将 JSON 转化为 Data URL 来构造 URL
- 通过将 JSON 转换为 `Blob` 再转化为 `Object URL` 来构造 URL

首先，我们需要创建一个 `a` 标签来模拟点击下载文件：

```js
// <a href="..." download>
function download(url, name) {
  const a = document.createElement('a')
  a.download = name
  a.rel = 'noopener'
  a.href = url
  // 触发模拟点击
  a.dispatchEvent(new MouseEvent('click'))
  // 或者 a.click()
}
```

## 使用 Data URL

```js
// JSON 数据
const json = {
  name: 'O.O',
  age: 20
}
const str = JSON.stringify(json, null, 2)

const dataURL = `data:,${str}`
download(dataURL, 'test.json')
```

## Blob 和 Object URL

```js
const url = URL.createObjectURL(new Blob(str.split('')))
download(url, 'test.json')
```
