# Surge 概览

Surge 是为开发者设计的网络开发和代理工具，因此需要专业知识才能使用。

Surge 的核心工作流由四项主要能力组成：

*   接管：Surge 允许用户接管设备发送的网络连接。支持代理服务和虚拟网卡接管。
*   处理：该软件允许用户修改被接管的网络请求和响应。包括 URL 重定向、本地文件映射、使用 JavaScript 进行自定义修改等多种方法。
*   转发：网络请求被接管后，用户可以将其转发到其他代理服务器。转发可以是全局的，也可以使用灵活的规则系统来确定出站策略。
*   截获：用户可以截获并保存网络请求和响应中的特定数据。此外，用户还可以通过 MITM 解密 HTTPS 流量。

## 特性

*   高性能、稳定、高效：Surge 能够以工业级稳定性，占用最少的系统资源，流畅处理所有网络流量。
*   灵活的规则系统：你可以基于域名、IP CIDR、GeoIP 等编写转发规则。Surge 可以自动使用 HTTP/HTTPS/SOCKS5/SOCKS5-TLS/Shadowsocks 协议将请求代理到其他服务器。
*   HTTPS 解密：通过中间人攻击解密 HTTPS 流量。证书生成器将帮助你生成受操作系统信任的 CA 证书，以便用于调试。
*   本地 DNS 映射：Surge 支持本地自定义 DNS 映射。它的多个功能模块，包括通配符、别名和自定义 DNS 服务器，能够满足各种需求。
*   策略组：你可以将多个代理归为一组，并根据分组采用相应的策略。策略组可以配置为自动测速（基于访问目标 URL 的速度选择策略）、SSID（基于 Wi-Fi SSID 选择策略）和手动选择。
*   HTTP 重写：你可以使用自定义规则将 HTTP/HTTPS 请求重写到另一个 URL，或者阻止这些请求；
*   远程面板：Surge Dashboard 可以通过 USB 或网络连接到远程的 Surge iOS 或 Surge Mac 实例。
*   完整的 IPv6 支持：所有功能都可在 IPv6 环境下工作。

### Surge Mac 独占特性

*   增强模式：对于未显式支持 Web 代理的应用程序，Surge 可以设置一个虚拟网络接口来处理所有网络流量。
*   计费网络模式：你可以控制允许哪些应用程序/进程访问互联网，这在使用计费连接（如蜂窝网络）时非常有用。
*   网关模式：Surge Mac 可以配置为三层网关，以处理同一网络中其他设备的网络流量。

### Surge iOS 独占特性

*   所有功能均可在蜂窝网络上使用。
*   捕获设备上任何应用的所有 HTTP/HTTPS/TCP 流量，并根据高度可配置的规则将其重定向到 HTTP/HTTPS/SOCKS5/Shadowsocks 代理服务器，即使应用程序不遵循系统代理设置也能生效。
*   即使在蜂窝网络下也能覆盖系统 DNS 设置，并通过同时查询所有 DNS 服务器来提升性能。
*   通过 Wi-Fi 或 USB 线缆将 Surge Dashboard 连接到 Surge iOS，监控和分析 iOS 设备上的网络请求。通过 USB 连接时，你甚至可以检查蜂窝网络请求。

### 深入理解 Surge

我们发布了一本官方指南以帮助你了解 Surge。

*   英文版：[https://manual.nssurge.com/book/understanding-surge/en/](https://manual.nssurge.com/book/understanding-surge/en/)
*   中文版：[https://manual.nssurge.com/book/understanding-surge/cn/](https://manual.nssurge.com/book/understanding-surge/cn/)
