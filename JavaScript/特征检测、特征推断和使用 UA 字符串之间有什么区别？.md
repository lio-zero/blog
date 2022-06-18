# 特征检测、特征推断和使用 UA 字符串之间有什么区别？

## 特征检测（feature detection）

特征检测包括确定浏览器是否支持某段代码，以及是否运行不同的代码（取决于它是否执行），以便浏览器始终能够正常运行代码功能，而不会在某些浏览器中出现崩溃和错误。例如：

```js
if ('geolocation' in navigator) {
  // 可以使用 navigator.geolocation
} else {
  // 处理 navigator.geolocation 功能缺失
}
```

[Modernizr](https://modernizr.com/) 是处理功能检测的优秀工具。

## 特征推断（feature inference）

特征推断与特征检测一样，会对功能可用性进行检查，但是在判断通过后，还会使用其他功能，因为它假设其他功能也可用，例如：

```js
if (document.getElementsByTagName) {
  elem = document.getElementById(id)
}
```

非常不推荐这种方式。特征检测更能保证万无一失。

## UA 字符串

**UA（User-Agent）字符串**，是浏览器报告的字符串，它允许网络协议对等方（network protocol peers）识别请求用户代理的应用类型、操作系统、应用供应商和应用版本。

它可以通过 `navigator.userAgent` 访问，例如 Chrome 将输出：

```js
console.log(navigator.userAgent)

// "Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/90.0.4430.93 Safari/537.36"
```

然而，这个字符串很难解析并且很可能存在欺骗性。例如，Chrome 会同时作为 Chrome 和 Safari 进行报告。

因此，要检测 Safari，除了检查 Safari 字符串，还要检查是否存在 Chrome 字符串。不要使用这种方式。
