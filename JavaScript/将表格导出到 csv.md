# 将表格导出到 csv

假设我们有一个 `table` 元素和一个 `button`，用于将表格单元格导出为 CSV，如下所示：

```html
<table id="exportMe" class="table">
  ...
</table>

<button id="export">Export</button>
```

## 将表格单元格导出为 CSV

下面的方法将 `table` 的所有单元格导出为 CSV 格式。首先，我们选择所有行，在它们上面循环并将每一行导出到 CSV。

在每一行中，我们遍历所有单元格，并使用 `textContent` 检索它们的文本内容。

```js
const toCsv = function (table) {
  const rows = table.querySelectorAll('tr')

  return [].slice
    .call(rows)
    .map(function (row) {
      const cells = row.querySelectorAll('th, td')
      return [].slice
        .call(cells)
        .map(function (cell) {
          return cell.textContent
        })
        .join(',')
    })
    .join('\n')
}
```

## 下载 CSV

下面的方法创建内容为 `text` 的伪 `a` 元素，并触发 `click` 事件。

```js
const download = function (text, fileName) {
  const link = document.createElement('a')
  link.setAttribute(
    'href',
    `data:text/csv;charset=utf-8,${encodeURIComponent(text)}`
  )
  link.setAttribute('download', fileName)

  link.style.display = 'none'
  document.body.appendChild(link)

  link.click()

  document.body.removeChild(link)
}
```

最后一件事是把所有的部件连接在一起。我们只需要处理导出按钮的 `click` 事件：

```js
const table = document.getElementById('exportMe')
const exportBtn = document.getElementById('export')

exportBtn.addEventListener('click', function () {
  // 导出到 csv
  const csv = toCsv(table)

  // 下载它
  download(csv, 'download.csv')
})
```

> [查看效果](https://codepen.io/lio-zero/pen/KKvvbbo)
