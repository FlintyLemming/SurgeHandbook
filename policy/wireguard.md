# WireGuard

你可以将 Surge 作为 WireGuard 客户端使用，把 L3 VPN 转换为出站代理策略。

```ini
[Proxy]
wireguard-home = wireguard, section-name = HomeServer

[WireGuard HomeServer]
private-key = sDEZLACT3zgNCS0CyClgcBC2eYROqYrwLT4wdtAJj3s=
self-ip = 10.0.2.2
self-ip-v6 = fd00:1111::11
dns-server = 8.8.8.8, 2606:4700:4700::1001
prefer-ipv6 = false
mtu = 1280
peer = (public-key = fWO8XS9/nwUQcqnkfBpKeqIqbzclQ6EKP20Pgvzwclg=, allowed-ips = 0.0.0.0/0, endpoint = 192.168.20.6:51820)
```

配置注意事项：

1.  所有密钥均可以使用 Base64 或 HEX 形式。
2.  你可以同时配置 `self-ip` 和 `self-ip-v6` 以利用 IPv4 & IPv6 双栈，或仅配置其中之一以使用单栈。
3.  请注意，每个设备的 `self-ip` 和 `self-ip-v6` 必须不同，否则可能导致 IP 抢占。
4.  如果 `prefer-ipv6` 为 true，在启用 IPv4 & IPv6 双栈并且域名同时配置了 A 和 AAAA 记录时，将优先使用 IPv6。
5.  `peer` 字段可以配置多个节点，使用逗号分隔，并以 `()` 表示一个节点。如果通过 UI 配置，则只允许配置单个节点。
6.  `preshared-key` 和 `keepalive` 是对端 (peer) 的可选参数。
7.  对端的 `endpoint` 可以使用域名。请注意，端点的解析由 `[General]` 段落配置的 DNS 解析器完成，并且与该段落中的 `dns-server` 参数无关。
8.  `0.0.0.0/0` 的 `allowed-ips` 意味着该策略可以用来访问任何地址，或者也可以配置为特定的内网地址。
9.  如果你需要通过此策略使用域名访问主机，你必须配置 `dns-server`，Surge 将通过 WireGuard VPN 隧道向该服务器进行 DNS 解析。可以配置多个 DNS 地址，使用逗号分隔。
10. `[WireGuard NAME]` 段落可以拆分到分离的配置段落文件 (Detached Profile Section file) 中。
11. 可以同时配置并使用多个 Wireguard 实例。

使用注意事项：

1.  WireGuard 是一种 L3 VPN，因此在处理期间的开销明显高于其他通用代理协议。它适用于带宽要求较低的场景。
2.  隧道仅支持 TCP 和 UDP 协议。此外，还提供了一种非常简单的 ICMP/ICMPv6 响应机制。WireGuard 握手成功时，可以从服务器端 ping 客户端隧道的 IP，以测试连通性。
3.  由于 WireGuard 协议没有报错机制，在大多数情况下，WireGuard 错误均表现为超时（例如密钥错误、防火墙拦截、服务端未配置 NAT 等），因此请自行抓包分析原因。

按照 WireGuard 协议标准的建议，WireGuard 握手数据包将被打上 0x88 (AF41) DSCP 标记，以提高成功率。

#### 自定义保留位 (Customize Reserved Bits) iOS 5.3.1+ Mac 4.10.3+

Surge 支持自定义 WireGuard 的保留位。它可能会被某些实现用作客户端 ID 或路由 ID，例如 Cloudflare WARP。

示例：

`peer = (public-key = <key>, allowed-ips = "0.0.0.0/0, ::/0", endpoint = example.com:51820, client-id = 83/12/235)`

#### ECN 支持 iOS 5.8.0+ Mac 5.4.0+

当通过 WireGuard 转发 UDP 数据包时，支持保留隧道内数据包的 TOS (DSCP/ECN) 标记。

根据 WireGuard 协议标准的建议，Surge 会将 ECN 标记从隧道内的数据包复制到外部数据包中。在接收带有 ECN 标记的数据包时，将根据 RFC6040 严格合并。（必须为 WireGuard 策略设置 `ecn=true`）。
