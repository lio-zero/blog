# Speech Synthesis API

[Speech Synthesis API](https://developer.mozilla.org/zh-CN/docs/Web/API/SpeechSynthesis) 是 Web Speech API 的一部分，同时也是 Speech Recognition API 的一部分，尽管目前仅在实验模式下在 Chrome 上支持。

[Speech Synthesis API 的支持情况](https://caniuse.com/?search=Speech%20Synthesis%20API)：

![Speech Synthesis API 的支持情况](https://upload-images.jianshu.io/upload_images/18281896-b2fdec23f8d8ac94.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 基本用法

使用 Speech Synthesis API 的最简单示例仅限于一行：

```js
speechSynthesis.speak(new SpeechSynthesisUtterance('你好'))
```

将其复制并粘贴到您的浏览器控制台中，您的将听到这句话！

## API

`SpeechSynthesisUtterance` 代表语音请求。在上面的示例中，我们传递了一个字符串。这是浏览器应该大声读出的信息。

获得语音对象后，可以执行一些调整来编辑语音属性：

```js
const utterance = new SpeechSynthesisUtterance('你好')
```

`SpeechSynthesis` 接口在 `window` 对象上可用。它的方法有 `speak()`、`pause()`、`cancel()`、`resume()` 和 `getVoices()`：

- `speak()` —— 添加一个 utterance 到语音谈话队列；它将会在其他语音谈话播放完之后播放。
- `pause()` —— 把 SpeechSynthesis 对象置为暂停状态。
- `cancel()` —— 移除所有语音谈话队列中的谈话。
- `resume()` —— 把 SpeechSynthesis 对象置为一个非暂停状态：如果已经暂停了则继续。
- `getVoices()` —— 返回当前设备所有可用声音的 SpeechSynthesisVoice 列表。

`SpeechSynthesisUtterance` 的实例属性有：

- `utterance.rate` — 设置速度，接受 [0.1 - 10] 之间，默认为 1
- `utterance.pitch` — 设置音高，接受 [0 - 2] 之间，默认为 1
- `utterance.volume` — 设置音量，接受 [0 - 1] 之间，默认为 1
- `utterance.lang` — 设置语言（值使用 BCP 47 语言标记，比如 en-US 或 it-IT）
- `utterance.text` — 您可以将其作为属性传递，而不是在构造函数中进行设置。文本最多可包含 32767 个字符
- `utterance.voice` — 设置语音

示例：

```js
const utterance = new SpeechSynthesisUtterance('你好')
utterance.pitch = 1.5
utterance.volume = 0.5
utterance.rate = 8
speechSynthesis.speak(utterance)
```

下文是上面一些方法更详细的讲解。

## 设置声音

浏览器有不同数量的可用语音。要查看列表，请使用以下代码：

```js
console.log(`Voices #: ${speechSynthesis.getVoices().length}`)

speechSynthesis.getVoices().forEach((voice) => {
  console.log(voice.name, voice.lang)
})
```

![可用语音列表](https://upload-images.jianshu.io/upload_images/18281896-67b64a6ae806c0d5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 跨浏览器实现以获取语言

由于我们有这种差异，我们需要一种方法来抽象它，以使用 API。本例进行了以下抽象：

```js
const getVoices = () => {
  return new Promise((resolve) => {
    let voices = speechSynthesis.getVoices()
    if (voices.length) {
      resolve(voices)
      return
    }

    speechSynthesis.addEventListener('voiceschanged', () => {
      voices = speechSynthesis.getVoices()
      resolve(voices)
    })
  })
}

const printVoicesList = async () => {
  ;(await getVoices()).forEach((voice) => {
    console.log(voice.name, voice.lang)
  })
}

printVoicesList()
```

## 使用自定义语言

默认语音说英语。

通过设置 `lang` 属性，您可以使用任何想要的语言：

```js
let utterance = new SpeechSynthesisUtterance('中国')
utterance.lang = 'zh-CN'
speechSynthesis.speak(utterance)
```

## 使用另一个声音

你可以有多个可用的声音选择：

```js
const lang = 'zh-CN'
const voiceIndex = 1

const speak = async (text) => {
  if (!speechSynthesis) return

  const message = new SpeechSynthesisUtterance(text)
  message.voice = await chooseVoice()
  speechSynthesis.speak(message)
}

const getVoices = () => {
  return new Promise((resolve) => {
    let voices = speechSynthesis.getVoices()
    if (voices.length) {
      resolve(voices)
      return
    }
    speechSynthesis.addEventListener('voiceschanged', () => {
      voices = speechSynthesis.getVoices()
      resolve(voices)
    })
  })
}

const chooseVoice = async () => {
  const voices = (await getVoices()).filter((voice) => voice.lang == lang)

  return new Promise((resolve) => {
    resolve(voices[voiceIndex])
  })
}

speak('点个关注不迷路')
```

[演示效果](https://codepen.io/lio-zero/pen/xxWwQdr)
