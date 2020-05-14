# Snell 服务端

Snell 是由我们团队开发的一个新的加密代理协议。你可以在这里找到服务端的代码：[https://github.com/surge-networks/snell](https://github.com/surge-networks/snell)

从 3.1.0 开始，你可以将 Surge macOS 版本身作为一个 Snell 代理服务器。在配置文件中加入如下字段：

```
[Snell Server]
interface = 0.0.0.0
port = 6160
psk = RANDOM_KEY_HERE
obfs = off
```