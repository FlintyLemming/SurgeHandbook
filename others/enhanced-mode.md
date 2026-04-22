# 增强模式 (Enhanced Mode - Surge Virtual Network Interface, VIF)

某些应用程序可能不遵循系统代理设置。在 Surge 中使用增强模式可以确保所有应用程序的流量都被处理。

为了实现这一点，Surge 创建了一个虚拟网络接口 (VIF) 并将其注册为默认路由。所有的 DNS 查询都会返回一个 198.18.0.0/15 网段中的虚拟 IP 作为响应。

需要注意的是，Surge VIF 只能处理 TCP、UDP 和 ICMP 流量。因此，仅在必要时启用此功能。此外，由于无法代理 ICMP 流量，Surge VIF 会直接返回响应。

增强模式在 Surge iOS 上默认启用，而在 Surge Mac 上，必须手动启动。

从 Surge Mac 5.8.0 开始，增强模式由 Apple 的 Network Extension 框架驱动，取代了旧版的 utun 驱动。配置文件中的旧参数（如 `vif-mode`）保留用于向后兼容，但不再影响运行时的行为。

# Surge VM 网关 (Surge VM Gateway)

### UDP 快速路径 (UDP Fast Path) Mac 6.4.0+

当 Surge Mac 运行在网关虚拟机 (Gateway VM) 模式时，创建数千个短生命周期 UDP 流的设备（如 P2P 下载器或网络游戏）可能会耗尽标准的四层代理引擎。UDP 快速路径功能会在这些高连接数客户端超过阈值（1 秒内 10 个连接或 10 秒内 30 个连接）时，自动将其降级为轻量级的 L3 转发模式。

*   通过快速路径转发的数据包完全绕过了代理引擎，因此无法被规则或 MITM 匹配。
*   低于 1024 的目标端口保持在正常模式，以保留与常用服务的兼容性。
*   如果你需要将客户端固定在某一种行为，可以从 Dashboard/设备列表中为每个客户端设备切换快速路径设置。
