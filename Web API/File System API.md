# File System API

[File System API](https://developer.mozilla.org/en-US/docs/Web/API/File_System_Access_API) 仅适用于基于 Chromium 的 Web 浏览器。

File System Access API 使读写用户文件和访问文件系统变得更加简单。

File System Access API 支持情況：

![File System Access API 支持情況](https://upload-images.jianshu.io/upload_images/18281896-6ea37298b56aff42.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

可以看到，它的支持情况不是很理想，仅在部分 Chromium 浏览器版本中得到支持。

下面是一个获取文本/图像文件的示例。

## 示例：获取文本/图像文件

```js
const hasSupport = () => Boolean('showOpenFilePicker' in window)
const buttonText = document.querySelector('#button-text')
const buttonImage = document.querySelector('#button-image')

buttonText.addEventListener('click', textUpload)
buttonImage.addEventListener('click', imageUpload)

async function textUpload() {
  const root = document.getElementById('api-filesystem')
  const contentText = root.querySelector('#content-text')
  const contentImage = root.querySelector('#content-image')
  const fileName = root.querySelector('#filename')

  try {
    let [fileHandle] = await window.showOpenFilePicker({
      types: [
        {
          description: 'Text Files',
          accept: {
            'text/plain': ['.txt']
          }
        }
      ]
    })
    let file = await fileHandle.getFile()
    let content = await file.text()

    root.style.display = 'flex'

    contentText.innerText = content
    contentText.style.display = 'block'

    fileName.innerText = file.name
    contentImage.innerHTML = ''
  } catch (error) {
    console.log('文件选择器已取消', error)
  }
}

async function imageUpload() {
  const root = document.getElementById('api-filesystem')
  const contentText = root.querySelector('#content-text')
  const contentImage = root.querySelector('#content-image')
  const fileName = root.querySelector('#filename')

  try {
    let [fileHandle] = await window.showOpenFilePicker({
      types: [
        {
          description: '图像文件',
          accept: {
            'image/*': ['.gif', '.jpeg', '.jpg', '.png', '.webp']
          }
        }
      ]
    })
    let file = await fileHandle.getFile()
    let imageURL = URL.createObjectURL(file)

    contentImage.innerHTML = ''

    const img = document.createElement('img')
    img.setAttribute('src', imageURL)
    img.setAttribute('alt', file.name)
    img.style.height = 'auto'
    img.style.width = '100%'

    root.style.display = 'flex'

    contentImage.appendChild(img)
    fileName.innerText = file.name
    contentText.innerText = ''
    contentText.style.display = 'none'
  } catch (error) {
    console.log('图像选择已取消。', error)
  }
}
```

[查看效果](https://codepen.io/lio-zero/pen/GRxpYoW) — 在 CodePen 上代码内嵌在 `iframe` 内，导致无法访问执行成功，将其复制到本地进行测试。

由于 File System Access API 兼容性不佳，如果您想在项目中使用，建议使用 [browser-fs-access](https://github.com/GoogleChromeLabs/browser-fs-access) 库，其提供了简洁的 API 和优雅降级方案。

## 进一步阅读

[The File System Access API: simplifying access to local files](https://web.dev/file-system-access/)
