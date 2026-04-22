# 通用策略参数

### 出站参数

以下所有参数对内置策略和代理策略均可用。

#### `interface` (默认值: 自动)

强制使用指定的出站网络接口。

```ini
ProxyHTTP = http, 1.2.3.4, 443, username, password, interface = en2
```

Direct 策略的别名也像代理策略一样支持 `interface` 参数。

```ini
[Proxy]
Corp-VPN = direct, interface = utun0
WiFi = direct, interface = en2, allow-other-interface=true
```

请确保该接口具有目标地址的有效路由表。

#### `allow-other-interface` (布尔值, 默认值: false)

当此选项为 true 时，如果所需的接口不可用，Surge 允许使用默认接口来绑定连接。否则，连接将直接失败。

```ini
ProxyHTTP = http, 1.2.3.4, 443, username, password, interface = en2, allow-other-interface=true
```

#### `dns-follow-interface` (布尔值, 默认值: false) iOS 5.15.2+ Mac 5.2.0+

让策略的 `interface` 参数对 DNS 查询也生效；匹配该策略的 DNS 请求将使用此接口进行查询。（如果在规则匹配阶段触发了 DNS 解析，则不会使用特定接口。）

#### `no-error-alert` (布尔值, 默认值: false)

不显示该策略的错误警告。

#### `ip-version`

选择在 IPv4 和 IPv6 协议之间的行为。此选项仅影响到代理服务器的连接。因此，只有当代理服务器的主机名是一个域名时，它才有意义。如果配置了底层代理，该选项无效，因为 DNS 解析发生在远程服务器上。

*   `dual` (默认，使用最快的链路)
*   `v4-only`
*   `v6-only`
*   `prefer-v4`
*   `prefer-v6`

#### `hybrid` (布尔值, 仅限 iOS, 默认值: false)

同时建立蜂窝数据和 Wi-Fi 连接，然后使用较快的链路。

#### `tfo` (布尔值, 默认值: false)

开启 TCP Fast Open。

#### `tos` (十进制或十六进制, 默认值: 0)

自定义 IP TOS 值。

#### `ecn` (布尔值, 默认值: false) iOS 5.8.0+ Mac 5.4.0+

开启 ECN (显式拥塞通知) 支持。它可以提高高丢包率环境下的带宽性能，但在不支持的网络环境中开启可能会导致连接失败。

#### `block-quic` iOS 5.8.0+ Mac 5.4.0+

通过代理转发 QUIC 流量可能会导致性能问题。开启此选项将阻断 QUIC 流量，使客户端回退到传统的 HTTPS/TCP 协议。

*   `auto`: 根据代理是否适合转发 QUIC 流量自动开启。
*   `on`: 阻断 QUIC 流量。
*   `off`: 不阻断 QUIC 流量。

### 测试

#### `test-url`

示例：`test-url=http://google.com`

覆盖全局的测试 URL。该 URL 用于可用性和延迟测试。Surge 通过向该 URL 发送 HTTP HEAD 请求来测试代理并进行基准测试。

#### `test-timeout` (单位：秒)

覆盖全局的测试超时时间。

#### `test-udp`

示例：`test-udp=google.com@1.1.1.1`

覆盖代理的全局 `proxy-test-udp` 设置。Surge 通过执行 DNS 查询来测试和基准测试 UDP 中继。
