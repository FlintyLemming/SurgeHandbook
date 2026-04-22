# 组件

Surge 包含几个组件。

### Surge 代理服务器

这是 Surge 的核心部分。它是一个全功能的 HTTP/SOCKS5 代理服务器，具有极高的性能和稳定性，使用 Objective-C 编写，并针对 macOS 和 iOS 进行了优化。

### Surge 虚拟网卡 (Surge VIF)

一些应用不遵循系统代理设置（如 Mail.app），因为它们需要使用原始 TCP 套接字。这类流量可以由 Surge VIF 处理。

Surge VIF 在 Surge iOS 上默认启用。你可以通过打开增强模式在 Surge Mac 上启用 Surge VIF。

这是 Surge iOS 的架构：![](../assets/Surge-Architecture.png)

### Surge Dashboard (仅限 Mac 版)

Surge Dashboard 是一个图形用户界面，用于审查和检查请求，并列出 DNS 缓存。它可以连接到本地 Surge 实例，或者在配置了外部控制器访问权限时连接到远程实例。
