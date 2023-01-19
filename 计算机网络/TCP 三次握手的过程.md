# TCP 三次握手的过程

由于 TCP 是面向连接的协议，因此需要先建立连接，然后两个设备才能进行通信。TCP 使用称为三向握手的过程来协商序列和确认字段并启动会话。这是该过程的图形表示：

![三次握手](https://upload-images.jianshu.io/upload_images/18281896-6dc07a0167af98f8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

三次握手（Three-way Handshake）过程如下：

- 客户端发送一个 SYN（Synchronize）报文给服务端，表示客户端准备建立连接。
- 服务端收到 SYN 报文后，回应一个 SYN + ACK（Synchronize-Acknowledgment）报文给客户端，表示服务端同意建立连接。
- 客户端收到 SYN + ACK 报文后，回应一个 ACK（Acknowledgment）报文给服务端，表示客户端已经收到了服务端的回应，连接建立成功。

数据传输过程完成后，TCP 将终止两个端点之间的连接。这个四步过程如下图所示：

![四次握手](https://upload-images.jianshu.io/upload_images/18281896-8e75dcd7f400cb27.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

四次握手（Four-way Handshake）过程如下：

- 客户端发送一个 FIN（Finish）报文给服务端，表示客户端准备关闭连接。
- 服务端收到 FIN 报文后，回应一个 ACK 报文给客户端，表示服务端已经收到了客户端的请求。
- 服务端在结束所有数据传输后，也发送一个 FIN 报文给客户端，表示服务端准备关闭连接。
- 客户端收到服务端的 FIN 报文后，回应一个 ACK 报文给服务端，表示客户端已经收到了服务端的请求。

这种三次握手和四次握手的过程确保了连接的可靠性，避免了数据丢失和错误。

## 更多资料

我们只不过是简单总结了这个过程，如果你想了解其中详细的内容，有任何疑惑可以查阅以下文章：

- [TCP three-way handshake](https://study-ccna.com/tcp-three-way-handshake/)
- [面试官问到三次握手，我甩出这张脑图，他服了！](https://juejin.cn/post/6844904132071948295)
- [TCP 的三次握手和四次挥手，了解泛洪攻击么](https://github.com/sisterAn/blog/issues/105)
- [TCP 协议灵魂之问，巩固你的网路底层基础](https://juejin.cn/post/6844904070889603085)
