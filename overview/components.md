# 组件

Surge 由几个组件构成

## **Surge Proxy Server**

这是 Surge 最核心的部分，由于使用 Objective-C 编写，并对 macOS 和 iOS 分别进行了优化，所以可以高效且稳定地作为 HTTP/SOCKS5 代理服务器处理所有请求。

## **Surge Virtual Network Interface \(Surge VIF\)**

有些应用不遵从系统代理设置（比如邮件），因为他们需要使用原生 TCP socket。Surge VIF 可以处理这些流量。

在 iOS 种，这项功能是默认被开启的；而在 macOS 中，可以通过打开 增强模式 使用该功能。

以下是 Surge iOS 版的结构：

![](https://manual.nssurge.com/Surge-Architecture.png)

## **Surge Dashboard （macOS 版独有功能）**

Surge Dashboard（在新版被译为“请求查看器”）是一个用于查看请求和 DNS 缓存的图形化界面。当开启远程访问时（设置 - 通用 - 远程 Dashboard），可以通过其他设备查看该设备的 Dashboard 信息。

