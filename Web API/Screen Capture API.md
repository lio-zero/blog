# Screen Capture API

[Screen Capture API](https://developer.mozilla.org/zh-CN/docs/Web/API/Screen_Capture_API) 对现有的媒体捕获和流 API 进行了补充，让用户选择一个屏幕或屏幕的一部分（如一个窗口）作为媒体流进行捕获。然后，该流可以被记录或通过网络与他人共享。

```js
const video = document.getElementById('screen-video')
const hasSupport = () => Boolean('mediaDevices' in navigator)

async function onCapture() {
  video.srcObject = await navigator.mediaDevices.getDisplayMedia({
    video: {
      aspectRatio: 16 / 9
    },
    audio: {
      echoCancellation: true,
      sampleRate: 44100
    }
  })

  const track = video.srcObject.getVideoTracks()[0]

  console.info(JSON.stringify(track.getSettings(), null, 2))
  console.info(JSON.stringify(track.getConstraints(), null, 2))
}

function onStop() {
  const video = document.getElementById('screen-video')

  video.srcObject.getTracks().forEach((stream) => stream.stop())

  video.srcObject = null
}

startCapture.addEventListener('click', () => {
  onCapture()
})

stopCapture.addEventListener('click', () => {
  onStop()
})
```

[示例地址](https://codepen.io/lio-zero/pen/KKowOrG)

## 更多资料

- [WebRTC-Experiment](https://github.com/muaz-khan/WebRTC-Experiment) — 用于音频/视频以及屏幕活动记录的 WebRTC JavaScript 库
- [在浏览器中截屏](https://github.com/lio-zero/blog/blob/main/JavaScript/%E5%9C%A8%E6%B5%8F%E8%A7%88%E5%99%A8%E4%B8%AD%E6%88%AA%E5%B1%8F.md)
