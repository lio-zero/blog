# 在 Node.js 中使用 sharp 处理图像

[sharp](https://github.com/lovell/sharp) 是高性能 Node.js 图像处理库。它的典型用例是将普通格式的大图像转换为较小的、适合 web 的 JPEG、PNG、WebP、GIF 和 AVIF 图像。

详细介绍请查阅 [README](https://github.com/lovell/sharp#sharp)。

本文将介绍它的一些用法。

> 以下默认已在项目安装、导入。

## 将图像转换为灰色

```js
const convertTograyscale = () => {
  sharp('./github.png')
    .grayscale() // or .greyscale()
    .toFile(__dirname + './grayImage.png')
}

convertTograyscale()
```

![将图像转换为灰色](https://upload-images.jianshu.io/upload_images/18281896-6c64cbb9c476f279.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

本文所有的案例所使用的[图片地址](https://repository-images.githubusercontent.com/301573344/43d44d72-0463-4109-b614-72bea18fd17e)，下文将值贴出代码，感兴趣的可以尝试尝试。

## 为图像着色

```js
const tintImage = () => {
  sharp('./github.png')
    .tint({ r: 255, g: 0, b: 0 })
    .toFile(__dirname + './tintImage.png')
}

tintImage()
```

## 提取图像元数据

```js
const imageMetadata = async () => {
  const metadata = await sharp('./github.png').metadata()

  console.log(metadata)
  /*
    {
      format: 'png',
      width: 1022,
      height: 640,
      space: 'srgb',
      channels: 4,
      depth: 'uchar',
      density: 144,
      isProgressive: false,
      hasProfile: true,
      hasAlpha: true,
      exif: <Buffer 4d 4d 00 2a 00 00 00 08 00 05 01 12 00 03 00 00 00 01 00 01 00 00 01 1a 00 05 00 00 00 01 00 00 00 4a 01 1b 00 05 00 00 00 01 00 00 00 52 01 28 00 03 ... 100 more bytes>,
      icc: <Buffer 00 00 0d 4c 61 70 70 6c 02 10 00 00 6d 6e 74 72 52 47 42 20 58 59 5a 20 07 e6 00 05 00 1f 00 0b 00 11 00 06 61 63 73 70 41 50 50 4c 00 00 00 00 41 50 ... 3354 more bytes>,
      xmp: <Buffer 3c 78 3a 78 6d 70 6d 65 74 61 20 78 6d 6c 6e 73 3a 78 3d 22 61 64 6f 62 65 3a 6e 73 3a 6d 65 74 61 2f 22 20 78 3a 78 6d 70 74 6b 3d 22 58 4d 50 20 43 ... 657 more bytes>
    }
  */
}

imageMetadata()
```

## 旋转图像

```js
const rotateImage = () => {
  sharp('./github.png')
    .rotate(180)
    .toFile(__dirname + './rotateImage.png')
}

rotateImage()
```

## 调整图像大小

```js
const resizeImage = async () => {
  const resize = await sharp('./github.png')
    .resize(350, 260)
    .toFile(__dirname + './resizeImage.png')

  console.log(resize)
  /*
    {
      format: 'jpeg',
      width: 350,
      height: 260,
      channels: 3,
      premultiplied: true,
      size: 10534
    }
  */
}

resizeImage()
```

## 格式化图像

```js
const formatImage = () => {
  sharp('./github.png')
    .toFormat('png', { palette: true })
    .toFile(__dirname + './formatImage.png')
}

formatImage()
```

## 裁剪图像

```js
const cropImage = () => {
  sharp('./github.png')
    .extract({ left: 490, top: 120, width: 380, height: 380 })
    .toFile(__dirname + './cropImage.png')
}

cropImage()
```

## 创建合成图像

```js
const compositeImage = () => {
  sharp('./resizeImage.png')
    .composite([{ input: './github.png', left: 100, top: 50 }])
    .toFile(__dirname + './compositeImage.png')
}

compositeImage()
```

## 模糊图像

```js
const blurImage = () => {
  sharp('./github.png')
    .blur(9)
    .toFile(__dirname + './blurImage.png')
}

blurImage()
```

## 锐化图像

```js
const sharpenImage = () => {
  sharp('./github.png')
    .sharpen(13)
    .toFile(__dirname + './sharpenImage.png')
}

sharpenImage()
```

## 翻转图像

```js
const flipImage = async () => {
  await sharp('./github.png')
    .flip()
    .toFile(__dirname + './flipImage.png')
}

flipImage()
```

## 镜像图像

```js
const flopImage = async () => {
  await sharp('./github.png')
    .flop()
    .toFile(__dirname + './flopImage.png')
}

flopImage()
```

## 向图像添加文本

```js
const addText = () => {
  const width = 100
  const height = 40
  const text = 'GitHub'

  const svgText = `
  <svg width="${width}" height="${height}">
    <style>
      .title { fill: red; font-size: 16px}
    </style>
    <text x="45%" y="40%" text-anchor="middle" class="title">${text}</text>
  </svg>`

  const svgBuffer = Buffer.from(svgText)

  sharp('./github.png')
    .composite([{ input: svgBuffer, left: 20, top: 40 }])
    .toFile(__dirname + './textImage.png')
}

addText()
```

## 更多资料

[How To Process Images in Node.js With Sharp](https://www.digitalocean.com/community/tutorials/how-to-process-images-in-node-js-with-sharp)
