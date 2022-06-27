# curl 和 wget 有什么区别？

主要区别在于：

- 与 `curl` 相比，`wget` 的主要优点是它能够递归下载。
- `wget` 只是命令行。
- `curl` 支持 FTP、FTPS、HTTP、HTTPS、SCP、SFTP、TFTP、TELNET、DICT、LDAP、LDAPS、FILE、POP3、IMAP、SMTP、RTMP 和 RTSP。`wget` 支持 HTTP、HTTPS 和 FTP。
- `curl` 构建和运行的平台比 `wget` 多。
- `wget` 是根据自由软件 copyleft 许可证（GNU GPL）发布的。`curl` 是根据自由软件许可证（MIT 衍生产品）发布的。
- `curl` 提供上传和发送功能。`wget` 只提供简单的 HTTP POST 支持。

来源 [What is the difference between curl and wget?](https://unix.stackexchange.com/questions/47434/what-is-the-difference-between-curl-and-wget)。
