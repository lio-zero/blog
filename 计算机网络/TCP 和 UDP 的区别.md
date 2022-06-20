# TCP 和 UDP 的区别

TCP 指[传输控制协议](https://zh.wikipedia.org/wiki/%E4%BC%A0%E8%BE%93%E6%8E%A7%E5%88%B6%E5%8D%8F%E8%AE%AE)（Transmission Control Protocol），而 UDP 指[用户数据报协议](https://zh.wikipedia.org/wiki/%E7%94%A8%E6%88%B7%E6%95%B0%E6%8D%AE%E6%8A%A5%E5%8D%8F%E8%AE%AE)（User Datagram Protocol），它们都运行在 IP（互联网协议）之上。是 [OSI](https://zh.wikipedia.org/wiki/OSI%E6%A8%A1%E5%9E%8B)（开放式系统互联模型）中的第 4 层协议。

- TCP 是一个面向连接的、可靠的、基于字节流的传输层协议，而 UDP 是一个面向无连接的（发送数据之前不需要先建立连接）、不可靠、基于报文的传输层协议。
- TCP 需要在网络接口级别进行更多处理，而在 UDP 中则不需要。
- TCP 使用 3 次握手、拥塞控制、流量控制等机制来保证可靠传输。
- UDP 主要用于数据包延迟比数据包丢失更严重的情况。
- TCP 比 UDP 慢。

## 更多资料

- [Difference between TCP and UDP?](https://stackoverflow.com/questions/5970383/difference-between-tcp-and-udp)
- [When is it appropriate to use UDP instead of TCP?](https://stackoverflow.com/questions/1099672/when-is-it-appropriate-to-use-udp-instead-of-tcp)
- [UDP vs TCP, how much faster is it? [closed]](https://stackoverflow.com/questions/47903/udp-vs-tcp-how-much-faster-is-it)
