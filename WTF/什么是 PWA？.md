# PWA

[PWA](https://zh.wikipedia.org/wiki/%E6%B8%90%E8%BF%9B%E5%BC%8F%E7%BD%91%E7%BB%9C%E5%BA%94%E7%94%A8%E7%A8%8B%E5%BA%8F)（Progressive Web App，渐进式 Web 应用程序）运用现代的 Web API 以及传统的渐进式增强策略来创建跨平台 Web 应用。这些应用无处不在、功能丰富，使其具有与原生应用相同的用户体验优势。

PWA 是一个术语，它标识了一组旨在为基于 Web 的应用创造更好体验的技术。

## 什么是 PWA？

PWA 是一种基于设备支持的应用，它可以提供额外的功能，当网站是安全的（使用 HTTPS），它们可以利用 [Service Worker](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API) 为用户提供离线支持，[推送通知](https://developer.mozilla.org/en-US/docs/Web/API/Push_API)可以帮助重新吸引用户，几乎原生的应用外观和速度，以及资源的本地缓存，而 [Web App Manifests](https://developer.mozilla.org/en-US/docs/Web/Manifest) 允许用户安装 PWA 和本地应用。

这项技术最初由 Google 于 2015 年推出，并被证明为开发人员和用户带来了许多好处。

开发人员可以使用 Web 堆栈构建几乎一流的应用，这通常比构建本地应用更容易、更便宜，尤其是在考虑构建和维护跨平台应用的影响时。

开发者可以从安装阻力的减少中获益，因为在商店中添加应用并不能为 99.99% 的应用带来任何曝光度，而搜索引擎可以提供同样的好处。

渐进式 Web 应用是一种使用特定技术开发的网站，使移动体验比普通的移动优化网站更愉快，在某种程度上，它几乎像原生应用一样工作，因为它提供了以下功能:

- 离线支持
- 加载速度
- 安全的
- 能够发出推送通知
- 在没有 URL 栏的情况下，即可获得沉浸式的全屏用户体验

移动平台对 PWA 提供了越来越多的支持，甚至当用户检测到正在访问的网站是 PWA 时，会要求用户将应用添加到主屏幕。

但首先，要稍微澄清一下名称。PWA 可能是一个令人困惑的术语，一个好的定义是 Web 应用利用现代浏览器的功能（如 Web Worker 和 Web App Manifest），让其移动设备将应用**升级**为一流公民应用。

## PWA 替代品

在构建移动体验方面，PWA 与其他替代方案相比如何？

让我们重点讨论每种方法的优缺点，看看 PWA 适合哪些地方。

### 原生移动应用

原生移动应用是构建移动应用最明显的方式。iOS 平台上的 Objective-C 或 Swift，Android 上的 Java/Kotlin 和 Windows Phone 上的 C#。

每个平台都有自己的 UI 和 UX 约定，原生小部件提供了用户期望的体验。它们可以通过平台应用商店进行部署和分发。

**原生应用的主要痛点是**，只能在一个平台上使用，而跨平台开发需要学习、掌握和更新许多不同的方法和最佳实践。因此，如果您有一个小团队，甚至您是在 3 个平台上开发应用的独立开发人员，您需要花费大量时间学习技术和环境，管理不同的库，并使用不同的工作流程（例如，iCloud 仅适用于 iOS 设备，没有 Android 版本）。

### 混合应用

混合应用使用 Web 技术构建，但可以部署到应用商店。中间是一个框架或某种方式来打包应用，因此可以将其发送到传统的应用商店进行审查。

最常见的平台是 Phonegap、Cordova、Ionic Framework 和许多其他平台，通常您在页面上看到的是一个 WebView，它实质上加载了一个本地 web 页面。

混合应用的关键方面是**一次编写、随处运行**的概念，不同的平台代码在构建时生成，您正在使用 JavaScript、HTML 和 CSS 构建应用，设备功能（麦克风、摄像头、网络、GPS...）通过 JavaScript API 公开。

**构建混合应用的坏处是**，除非您做得很好，否则您可能会选择提供一个共同点，有效地创建一个在所有平台上都不是最佳的应用，因为该应用忽略了特定于平台的人机交互准则.

此外，复杂视图的性能可能会受到影响。

### 跨平台应用

跨平台应用不使用 Web 技术，即它的页面不是 HTML5 页面，而是使用自己的语法写的 UI 层，然后编译成各平台的原生应用。

常见的有 React Native、Xamarin、Flutter 等。

以 React Native 为例。

React Native 通过 JavaScript API 公开移动设备的原生控件，但实际上是在创建原生应用，而不是在 WebView 中嵌入网站。

它将这种方法与混合应用区分开来，即**一次学习，随处编写**，这意味着跨平台的方法是相同的，但您要创建完全独立的应用，以便在每个平台上提供良好的体验。

性能与原生应用相当，因为您构建的本质上是一个原生应用，通过应用商店分发。

关于以上的内容，可以阅读阮一峰老师的 [H5 手机 App 开发入门：概念篇](https://www.ruanyifeng.com/blog/2019/12/hybrid-app-concepts.html)和 [H5 手机 App 开发入门：技术篇](https://www.ruanyifeng.com/blog/2019/12/mobile-app-technology-stack.html)。

## PWA 功能

在上一节中，您看到了 PWA 的主要竞争对手。那么 PWA 与它们相比如何，它的主要特点是什么？

### 特性

PWA 有一点与上述方法完全不同：**它们没有部署到应用商店**。

这是一个关键的优势，因为如果您有足够的影响力，那么应用商店是有益的，这可以让您的应用走红，但除非您处于 0.001%中，否则您不会从应用商店中获得太多的好处。

PWA 可以通过 SEO 发现，当用户访问具有 PWA 功能的网站时，浏览器和设备会询问用户是否想要将该应用安装到主屏幕上。这是非常重要的，因为常规的 SEO 可以应用于你的 PWA，从而减少对付费获取的依赖。

没有进入商店意味意味着你不需要通过各平台的批准就可以放入用户的口袋里，并且你可以随时发布更新内容，而不需要经过各平台标准审批流程。

PWA 基本上是增强型 HTML5 应用/响应式网站，最近引入的一些关键技术使一些关键功能成为可能。如果你还记得，最初的 iPhone 没有开发原生应用的选项，开发者被告知开发 HTML5 移动应用，可以安装到主屏幕上，但当时的技术还没有做好准备。

PWA 可以**离线运行**。使用 Service Worker 可以让应用始终具有最新的内容，并在后台下载，并支持推送通知，以提供更多的重新参与机会。

此外，可共享性为想要共享您的应用的用户提供了更好的体验，因为他们只需要一个 URL。

### 好处

**那么，用户和开发者为什么要关心 PWA 呢？**

- PWA 更轻量。原生应用的大小可以达到 100MB 或更多，而 PWA 可以在 KB 范围内
- 没有原生平台代码
- 降低获取成本（说服用户安装应用比说服用户访问网站获得首次体验要困难得多）
- 构建和发布更新所需的工作量大大减少
- 对深度链接的支持要比普通的应用商店应用多得多

### 核心概念

- **响应式** — UI 与设备屏幕大小相适应
- **接近原生应用** — 它不像一个网站，而是尽可能地成为一个应用
- **离线支持** — 它将使用设备存储提供离线体验
- **重新吸引用户** — 推送通知可帮助用户在安装应用后重新发现应用
- **保持最新内容** — 应用在线后会自动更新自身和内容
- **安全** — 通过 HTTPS，PWA 可以阻止通讯窃听，而且能保证内容不被篡改
- **渐进式** — 它可以在任何设备上运行，即使是旧设备，即使功能较少（例如，仅作为网站，不可安装）
- **可被安装** — 设备浏览器提示用户安装你的应用
- **可被发现** — 搜索引擎和 SEO 优化可以提供比应用商店更多的用户
- **可被链接** — 只需轻松地分享 URL 便可链接至 PWA 中，无需复杂的安装步骤

## Service Worker

PWA 的部分定义是它必须离线工作。

因为允许 Web 应用离线工作的是 Service Worker，这意味着 Service Worker 是 PWA 的必需组成部分。

> **Tips**：不要将 Service Worker 与 Web Worker 混淆，它们是完全不同的东西。

Service Worker 是一个 JavaScript 文件，充当 Web 应用和网络之间的中间人。正因为如此，它可以提供缓存服务，并加快应用的渲染速度，改善用户体验。

由于安全原因，只有 HTTPS 网站可以使用 Service Worker，这也是 PWA 必须通过 HTTPS 提供服务的部分原因。

用户第一次访问应用时，设备上的 Service Worker 不可用。第一次访问时，Web Worker 被安装了，然后在后续访问网站的不同页面时，会调用这个 Service Worker。

> 推荐：[Service Worker](https://github.com/lio-zero/blog/blob/main/Web%20API/Service%20Worker.md)。

## App Manifest

App Manifest 是一个 JSON 文件，可用于提供有关 PWA 的设备信息。

您可以在所有 Web 页面标题中添加指向 manifest 的链接：

```html
<link rel="manifest" href="/manifest.webmanifest" />
```

该文件将告诉设备如何设置：

- 应用的名称和简称
- 各种尺寸的图标位置
- 相对于域的起始 URL
- 默认方向
- 启动屏幕
- 快捷方式
- 屏幕截图

例如：

```json
{
  "name": "The Weather App",
  "short_name": "Weather",
  "description": "PWA 示例",
  "icons": [
    {
      "src": "images/icons/icon-128x128.png",
      "sizes": "128x128",
      "type": "image/png"
    },
    {
      "src": "images/icons/icon-144x144.png",
      "sizes": "144x144",
      "type": "image/png"
    },
    {
      "src": "images/icons/icon-152x152.png",
      "sizes": "152x152",
      "type": "image/png"
    },
    {
      "src": "images/icons/icon-192x192.png",
      "sizes": "192x192",
      "type": "image/png"
    },
    {
      "src": "images/icons/icon-256x256.png",
      "sizes": "256x256",
      "type": "image/png"
    }
  ],
  "shortcuts": [
    {
      "name": "Today's agenda",
      "url": "/today",
      "description": "List of events planned for today"
    },
    {
      "name": "New event",
      "url": "/create/event"
    }
  ],
  "screenshots": [
    {
      "src": "screenshot1.webp",
      "sizes": "1280x720",
      "type": "image/webp"
    },
    {
      "src": "screenshot2.webp",
      "sizes": "1280x720",
      "type": "image/webp"
    }
  ],
  "start_url": "/index.html?utm_source=app_manifest",
  "orientation": "portrait",
  "display": "standalone",
  "background_color": "#3E4EB8",
  "theme_color": "#2F3BA2"
}
```

- [`screenshots`](https://developer.mozilla.org/en-US/docs/Web/Manifest/screenshots) 定义了一组用于展示应用的屏幕截图。这些图像旨在供渐进式 Web 应用商店使用。
- [shortcut](https://developer.mozilla.org/en-US/docs/Web/Manifest/shortcuts) 定义指向 Web 应用中关键任务或页面的快捷方式或链接数组

另外，[App Manifest 是一份 W3C 工作草案](https://www.w3.org/TR/appmanifest/)。

## App Shell

[App Shell](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/App_structure#app_shell) 不是一种技术，而是一种设计概念，旨在首先加载和渲染 Web 应用容器，然后再加载实际内容，从而给用户一种类似于应用的良好印象。

这与 Apple HIG（人机界面指南）建议使用类似于用户界面的启动画面是一样的，这是一种心理暗示，可以降低应用加载时间过长的感觉。

### 缓存

[App Shell](https://developers.google.com/web/updates/2015/11/app-shell) 与内容分开缓存，它的设置使得从缓存中检索 shell 构建块需要很短的时间。

## 更多资料

- [Learn PWA](https://web.dev/learn/pwa/)
- [Progressive web app structure](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/App_structure)
- [小程序鼻祖 —— 在国内逐渐消亡的 PWA 可以带给我们哪些启示？](https://juejin.cn/post/7088496764331753502)
- [全平台的轻量体验：PWA 使用指南及应用推荐](https://zhuanlan.zhihu.com/p/362663779)
