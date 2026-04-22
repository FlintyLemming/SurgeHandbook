# `[General]` 段落中的杂项选项

由于选项经常变化，你可以在应用内查找关于 `[General]` 段落选项的最新解释。

*   Surge Mac: 主窗口菜单 -> 帮助 -> 配置语法 (Profile Syntax)
*   Surge iOS: 更多选项卡 -> 帮助 -> 配置语法 (Profile Syntax)

### loglevel

日志级别。可选值为：`verbose`、`info`、`notify` 或 `warning`。不建议在日常使用中启用 `verbose`，因为这会显着降低性能。

### ipv6

启用完整的 IPv6 支持。具体来说，启用此选项后，在访问域名时将查询域名的 AAAA 记录。即使未启用此选项，你也可以通过直接访问 IPv6 地址来访问 IPv6 站点。

### ipv6-vif

允许 IPv6 流量通过 Surge VIF。当你想让 Surge 处理连接到 IPv6 地址的原始 TCP 连接时非常有用。

*   `off`：从不在 Surge VIF 上设置 IPv6。
*   `auto`：仅当本地网络具有有效的 IPv6 网络时，才在 Surge VIF 上设置 IPv6。
*   `always`：始终在 Surge VIF 上设置 IPv6。

### dns-server

上游 DNS 服务器的 IP 地址。

### skip-proxy

在 iOS 版本中，此选项强制由 Surge VIF 处理对这些域名/IP 范围的连接，而不是由 Surge 代理引擎处理。在 macOS 版本中，当启用“设置为系统代理 (Set as System Proxy)”时，这些设置将应用于系统。此选项用于修复某些应用的兼容性问题。

*   要指定单个域名，请输入该域名 - 例如，apple.com。
*   要指定某个域上的所有网站，请在域名前使用星号 - 例如，\*apple.com。
*   要指定域名的特定部分，请分别指定每个部分 - 例如，store.apple.com。
*   要通过 IP 地址指定主机或网络，请输入特定的 IP 地址，如 192.168.2.11，或地址范围，如 192.168.2.\* 或 192.168.2.0/24。

注意：如果你输入的是 IP 地址或地址范围，你只能在使用该 IP 地址连接到该主机时绕过代理，而不能在通过解析为该地址的域名连接主机时绕过代理。

### exclude-simple-hostnames

与 skip-proxy 参数类似。此选项让直接使用简单主机名（不含点 `. `）的请求由 Surge VIF 处理，而不是 Surge 代理引擎。

### external-controller-access

此选项允许外部控制器控制 Surge，例如 Surge Dashboard (macOS) 和 Surge iOS Remote Controller (iOS)。例如：`key@0.0.0.0:6165`

### http-api

此选项允许使用 HTTP API 控制 Surge。例如：`key@0.0.0.0:6166`

### http-api-tls

使用 HTTPS 协议代替 HTTP。必须首先配置 MitM CA 证书。你需要手动将证书安装到客户端设备上。

### http-api-web-dashboard

启用此功能后，你可以通过 Web 浏览器控制 Surge。

### show-error-page Mac 5.8.0+

控制当请求失败时（例如因为被策略拒绝或代理无法连接）Surge 是否显示其内置的 HTTP 错误页面。该参数默认启用；如果你更希望客户端收到原始的网络错误而不是 Surge 错误页面，请将其设置为 `false`。

### show-error-page-for-reject

如果请求是纯 HTTP 请求，则为 REJECT 策略显示错误网页。

### full-header-mode

启用后，Surge 会将完整的 HTTP 标头数组（包括重复的标头字段）暴露给请求头重写规则、脚本和抓包。这有助于依赖多个相同标头名称的场景。默认模式为了兼容性仅保留最后一个值。

### tun-excluded-routes

Surge VIF 只能处理 TCP 和 UDP 协议。使用此选项来绕过特定的 IP 范围，允许所有流量通过。

注意：此选项仅适用于 Surge VIF。由 Surge 代理服务器 (Surge Proxy Server) 处理的请求不受影响。将 `skip-proxy` 和 `tun-excluded-routes` 结合使用，可以确保特定的 HTTP 流量绕过 Surge。

当启用 IPv6 VIF 时，支持 IPv6 CIDR 块。

### tun-included-routes

默认情况下，Surge VIF 接口将其自身声明为默认路由。然而，由于 Wi-Fi 接口拥有更小的路由，某些流量可能不会通过 Surge VIF 接口。使用此选项可添加更小的路由。

当启用 IPv6 VIF 时，支持 IPv6 CIDR 块。

### internet-test-url

用于测试互联网连通性的 URL。同时也是 DIRECT 策略的测试 URL。

### proxy-test-url

代理策略的默认测试 URL。

### test-timeout

连通性测试超时时间。

### always-real-ip

此选项要求 Surge 在 Surge VIF 处理 DNS 查询时返回真实 IP 地址，而不是虚假 IP 地址。

DNS 数据包将被转发至上游 DNS 服务器。

此参数属于 Host List 类型，详细规则请见：[Host List 参数类型](host-list.md)

### hijack-dns

默认情况下，Surge 仅对发送至 Surge DNS 地址 (198.18.0.2) 的 DNS 查询返回虚假 IP 地址。发送至标准 DNS 的查询将被转发。

某些设备或软件总是使用硬编码的 DNS 服务器。（例如，Google Speakers 总是使用 8.8.8.8）。你可以使用此选项劫持该查询以返回虚假地址。

你可以使用 `hijack-dns = *:53` 来劫持所有的 DNS 查询。

虚假 DNS 响应器监听在 `198.18.0.2` (IPv4) 和 `fd00:6152::2` (IPv6)，因此即使是纯 IPv6 网络也可以将其客户端指向 Surge。

### force-http-engine-hosts

让 Surge 将 TCP 连接视为 HTTP 请求。Surge 的 HTTP 引擎将处理这些请求，并且所有高级功能都将可用，例如抓包、重写和脚本。

此参数属于 Host List 类型，详细规则请见：[Host List 参数类型](host-list.md)

### encrypted-dns-follow-outbound-mode

默认情况下，加密 DNS 查询使用 DIRECT 出站。启用此选项可使 DOH 遵循出站模式设置和规则。

### encrypted-dns-server

加密 DNS 服务器的 URL。如果配置了加密 DNS，传统 DNS 将仅用于测试连通性和解析加密 DNS URL 中的域名。

支持的协议：

*   DNS over HTTPS: `https://example.com`
*   DNS over HTTP/3: `h3://example.com`
*   DNS over QUIC: `quic://example.com`

### encrypted-dns-skip-cert-verification

跳过加密 DNS 服务器证书验证，这并不安全。

### use-local-host-item-for-proxy

默认情况下，如果使用了代理策略，则始终在远程服务器上执行 DNS 查询。启用此选项后，如果目标域名的本地 DNS 映射结果存在，Surge 将使用该 IP 地址而不是域名来建立代理连接。

### geoip-maxmind-url

用于更新的 GeoIP 数据库的 URL。

### disable-geoip-db-auto-update

禁用 GeoIP 数据库的自动更新。

### allow-dns-svcb

iOS 系统可能会执行 SVCB 记录的 DNS 查询，而不是标准的 A 记录查询。这会导致 Surge 无法返回虚拟 IP 地址。因此，默认情况下，禁止进行 SVCB 记录查询，强制系统执行 A 记录查询。

### udp-policy-not-supported-behaviour

当 UDP 流量匹配到一个不支持 UDP 转发的策略时的回退行为。可选值为：`DIRECT`，`REJECT`。从 Surge Mac 6.0.0 开始，默认值为 `REJECT`，以避免在不知情的情况下泄漏流量。

### proxy-test-udp

代理的默认 UDP 测试参数。例如：`apple.com@8.8.8.8`

### udp-priority

启用后，在系统负载非常高且数据包处理出现延迟时，将优先处理 UDP 数据包。又称为游戏模式。

### always-raw-tcp-hosts iOS 5.8.0+ Mac 5.4.0+

Surge 将自动嗅探发送至端口 80 和 443 的 TCP 请求的协议，以启用高级 HTTP/HTTPS 功能，同时优化性能。然而，这可能会导致某些兼容性问题。如果你遇到问题，可以在此处添加主机名，Surge 将不会嗅探这些请求的协议。

此参数属于 Host List 类型，详细规则请见：[Host List 参数类型](host-list.md)

### always-raw-tcp-keywords Mac 5.5.0+

行为类似于 `always-raw-tcp-hosts`，但通过子串进行匹配而不是使用 Host List。任何包含其中一个关键字的主机名都将跳过协议嗅探并保持在原始 TCP 模式，这在精确主机名不可预测时非常有帮助。

### proxy-restricted-to-lan iOS 5.13.1+ Mac 5.8.1+

### gateway-restricted-to-lan iOS 5.13.1+ Mac 5.8.1+

发现有些用户由于缺乏网络安全知识，意外地将代理和网关服务暴露在互联网上（例如配置了 DMZ）。因此，添加了这两个参数，限制代理和网关服务仅接受来自当前子网设备的访问。这两个参数默认处于开启状态。

### icmp-forwarding iOS 5.14.3+ Mac 5.10.0+

在开启了增强模式后，为了减少对用户的干扰，Surge 会直接转发所有 ICMP 的包，从而不影响 ping 等工具的使用。

但这可能对于部分极度关注隐私的用户来说，会导致 IP 泄漏。所以新增了 icmp-forwarding 选项可以关闭该行为。

默认开启。

### block-quic iOS 5.14.6+ Mac 5.10.3+

该参数用于全局覆盖是否阻断 QUIC 流量的行为，可设置为：

*   `per-policy`：由 policy 的 `block-quic` 参数决定。这是默认值，与目前版本的行为一致。
*   `all-proxy`：覆盖代理策略的 `block-quic` 参数，全部阻断。
*   `all`：覆盖所有策略的 `block-quic` 参数，包括 DIRECT 策略也全部阻断。
*   `always-allow`：覆盖代理策略的 `block-quic` 参数，全部放行。

## 仅限 Surge iOS 参数

### allow-wifi-access

允许局域网内的其他设备访问 Surge 代理服务。

### wifi-access-http-port

Surge HTTP 代理服务的端口号。

### wifi-access-socks5-port

Surge SOCKS5 代理服务的端口号。

### wifi-access-http-auth

要求 Surge HTTP 代理服务进行身份验证。例如：`username:password`

### wifi-assist

启用 Wi-Fi 助理。

### hide-vpn-icon

隐藏状态栏中的 VPN 图标。

### all-hybrid

当 Wi-Fi 网络状况不佳时，不只是切换为使用蜂窝数据建立连接，而是始终同时使用 Wi-Fi 和蜂窝数据建立连接。

此选项可以在较差的 Wi-Fi 网络环境下或 Wi-Fi 网络正在切换时显着提升网络体验。

该功能将应用于所有 TCP 连接和 DNS 查询。仅当你的蜂窝数据套餐无限制时才启用此选项。

### allow-hotspot-access

当个人热点开启时，允许其他设备访问 Surge 代理服务。

### include-all-networks

默认情况下，某些请求可能不会被 Surge 接管。例如，应用可以绑定到物理网络接口以绕过 Surge VIF。启用包含所有网络 (Include All Networks) 选项可确保所有请求都由 Surge 处理且不会发生泄漏。当你将 Surge 用作防火墙时，此选项非常有用。（需要 iOS 14.0 或更高版本）

启用此选项可能会导致 AirDrop 和 Xcode 调试问题、通过 USB 连接的 Surge Dashboard 无法工作，以及其他意想不到的副作用。请谨慎使用。

### include-local-networks

启用此选项以使 Surge VIF 能够接管发送至局域网的请求。（需要 iOS 14.2 或更高版本）

启用此选项可能会导致 AirDrop 和 Xcode 调试问题、通过 USB 连接的 Surge Dashboard 无法工作，以及其他意想不到的副作用。请谨慎使用。

必须与 `include-all-networks=true` 结合使用。

### include-apns

启用此选项以使 Surge VIF 处理 Apple Push Notification service (APNs) 的网络流量。

必须与 `include-all-networks=true` 结合使用。

### include-cellular-services

启用此选项以使 Surge VIF 处理蜂窝服务的可路由到互联网的网络流量。（VoLTE、Wi-Fi 通话、IMS、MMS、可视化语音信箱等）

请注意，某些移动运营商直接将蜂窝服务流量路由至运营商网络，绕过了互联网。此类蜂窝服务流量始终被排除在隧道之外。

必须与 `include-all-networks=true` 结合使用。

### compatibility-mode

此选项用于控制 Surge iOS 的工作模式。

*   0: 自动，在低于 5.8.0 版本的 Surge iOS 中等效于 1，从 5.8.0 开始等效于 3。
*   1: Proxy Takeover + VIF，在该模式下，代理接管的优先级高于 VIF 接管，提供了最佳的性能，但有些应用程序可能会检查代理设置并拒绝工作。
*   2: 仅 Proxy Takeover
*   3: 仅 VIF Takeover：最新版本的默认工作模式。
*   4: Proxy Takeover + VIF，但代理使用 VIF 地址而不是回环地址。
*   5: 仅 VIF Takeover，但 VIF 路由使用多个较小的路由进行接管，不配置默认路由，可以用来绕过一些特殊问题。（例如：HomeKit 安防摄像头）

### auto-suspend iOS 5.11.0+

当检测到被 Surge Mac 接管的网络时，自动暂停 Surge iOS。默认开启。

## 仅限 Surge Mac 参数

### use-default-policy-if-wifi-not-primary

如果禁用此项，即使 Wi-Fi 不是主网络接口，SSID/BSSID 模式仍可匹配。

### read-etc-hosts

遵循 `/etc/hosts` 中的本地 DNS 映射项。

### http-listen

HTTP 代理服务监听参数。例如：`0.0.0.0:6152`

### socks5-listen

SOCKS5 代理服务监听参数。例如：`0.0.0.0:6153`

### debug-cpu-usage

启用 CPU 调试模式。这可能会降低性能。

### debug-memory-usage

启用内存调试模式。这可能会降低性能。
