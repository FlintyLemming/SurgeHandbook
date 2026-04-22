# 代理策略 (Proxy Policy)

代理策略指示将请求转发到另一个代理服务器。Surge 支持 HTTP/HTTPS/SOCKS5/SOCKS5-TLS 以及更多代理协议。

`[Proxy]` 段落用于声明代理策略。你可以为不同的规则创建多个代理。

配置行示例：

```ini
[Proxy]
ProxyHTTP = http, 1.2.3.4, 443, username, password
ProxyHTTPS = https, 1.2.3.4, 443, username, password
ProxySOCKS5 = socks5, 1.2.3.4, 443, username, password
ProxySOCKS5TLS = socks5-tls, 1.2.3.4, 443, username, password, skip-common-name-verify=true
```

## 代理类型

Surge 支持大多数常见的标准代理协议。

*   HTTP 代理: `ProxyHTTP = http, 1.2.3.4, 443, username, password`
*   HTTPS 代理 (基于 TLS 的 HTTP 代理): `ProxyHTTPS = https, 1.2.3.4, 443, username, password`
*   SOCKS5: `ProxySOCKS5 = socks5, 1.2.3.4, 443, username, password`
*   SOCKS5 over TLS: `ProxySOCKS5TLS = socks5-tls, 1.2.3.4, 443, username, password`
*   SSH
*   WireGuard (作为代理的 L3 层 VPN)

Surge 也支持几种流行的社区代理协议。

*   Snell: `Proxy-Snell = snell, 1.2.3.4, 8000, psk=password, version=4`
*   Shadowsocks: `Proxy-SS = ss, 1.2.3.4, 8000, encrypt-method=chacha20-ietf-poly1305, password=abcd1234`
*   VMess: `Proxy-VMess = vmess, 1.2.3.4, 8000, username=0233d11c-15a4-47d3-ade3-48ffca0ce119`
*   Trojan: `Proxy-Trojan = trojan, 192.168.20.6, 443, password=password1`
*   TUIC: `Proxy-TUIC = tuic, 192.168.20.6, 443, token=pwd, alpn=h3`
*   Hysteria 2: `Proxy-Hysteria = hysteria2, 192.168.20.6, 443, password=pwd, download-bandwidth=100` iOS 5.8.0+ Mac 5.4.0+
*   AnyTLS: `Proxy-AnyTLS = anytls, 192.168.20.6, 443, password=pwd` iOS 5.17.0+ Mac 6.4.3+

### UDP 转发

Surge 支持 SOCKS5、Snell v4/v5、Shadowsocks、Trojan、WireGuard、Hysteria 2 和 TUIC 协议的 UDP 转发。由于服务器并不总是支持 UDP 转发，Shadowsocks 和 SOCKS5 代理的 UDP 转发支持应通过添加参数 `udp-relay=true` 手动开启。

## 参数

#### 代理链 (Proxy Chain)

*   `underlying-proxy`

    使用一个代理来连接另一个代理，也称为代理链。它可以是另一个代理策略或策略组的名称。

#### 基于 TLS 的代理的通用参数 (HTTP, SOCKS5-TLS, VMess, Trojan, TUIC, Hysteria 2, AnyTLS)

*   `skip-cert-verify`: 可选，"true" 或 "false"（默认值：false）。

    如果启用此选项，Surge 将不验证服务器的证书。

*   `sni`: 默认值为代理的主机名

    你可以自定义 TLS 握手期间的 SNI (Server Name Indication)。使用 `sni=off` 可完全关闭 SNI。默认情况下，Surge 像大多数浏览器一样发送使用主机名的 SNI。

*   `server-cert-fingerprint-sha256`: 可选。

    使用固定的服务器证书，而不是标准的 X.509 验证。

#### HTTP/HTTPS 协议参数

*   `always-use-connect`: 可选。

    始终使用 HTTP CONNECT 方法来中继请求，即使是纯 HTTP 请求也是如此。

#### SOCKS5 协议参数

*   `udp-relay`: 可选。由于 SOCKS5 服务器的 UDP 转发是可选的，因此你必须明确启用 UDP 转发。

#### Snell 协议参数

有关更多信息，请参阅知识库 [Snell](https://kb.nssurge.com/surge-knowledge-base/release-notes/snell)。

*   `psk`: 必填。
*   `version`: 必填。
*   `reuse`: 可选。连接复用是 Snell V4 的一个可选功能。
*   `obfs`: 可选。`http` 是 Snell V4 唯一支持的混淆选项。
*   `obfs-host`: 可选。
*   `obfs-uri`: 可选。

#### Shadowsocks 协议参数

*   `udp-relay`: 可选。由于 Shadowsocks 服务器的 UDP 转发是可选的，因此你必须明确启用 UDP 转发。
*   `obfs`: 可选。`http` 或 `tls`。
*   `obfs-host`: 可选。
*   `obfs-uri`: 可选。
*   `udp-port`: 可选。进行 UDP 转发时，使用另一个服务器端口号。这可以在服务器的 TCP 和 UDP 服务不监听同一个端口时使用（例如配置了 ShadowTLS）。iOS 5.14.0+ Mac 5.9.0+

#### VMess 协议参数

*   `ws`: 可选。使用 WebSocket 传输层。
*   `ws-path`: 可选。
*   `ws-headers`: 可选。
*   `encrypt-method`: 可选。可能的值：`chacha20-ietf-poly1305` 或 `aes-128-gcm`。
*   `vmess-aead`: 可选。

#### Trojan 协议参数

*   `ws`: 可选。使用 WebSocket 传输层。
*   `ws-path`: 可选。
*   `ws-headers`: 可选。

#### TUIC 参数

*   `token`: 必填。
*   `alpn`: 可选。它必须匹配服务器的 ALPN 设置。
*   `port-hopping`: 可选。配置一个端口或端口范围的列表（例如 `1234;5000-6000`）。Surge 将在它们之间定期轮换，而不是使用主端口。
*   `port-hopping-interval`: 可选。轮换的间隔（秒），默认为 30。设置此字段时，声明中的主端口将被忽略。

#### Hysteria 2 参数 iOS 5.8.0+ Mac 5.4.0+

*   `download-bandwidth`: 可选，单位为 Mbps。
*   `port-hopping`: 可选。以分号分隔的显式端口或范围列表。启用发行说明中描述的端口跳跃模式。
*   `port-hopping-interval`: 可选。间隔（秒），默认为 30。启用端口跳跃后，前导的端口参数不再被使用。

#### AnyTLS v2 参数 iOS 5.17.0+ Mac 6.4.3+

*   `reuse`: 可选。根据 AnyTLS 规范，默认启用连接复用。你可以通过将 `reuse` 设置为 false 来禁用它。

## 用于 TLS 代理的客户端证书

Surge 支持对基于 TLS 的代理进行客户端证书验证。

示例：

```ini
[Proxy]
Proxy = https, example.com, 443, client-cert=cert1

[Keystore]
cert1 = base64=<此处为 P12 的 base64 字符串>, password=123456
```

## Shadow TLS

Shadow TLS 是一种代理混淆器，可与任何基于 TCP 的代理一起使用。([https://github.com/ihciah/shadow-tls](https://github.com/ihciah/shadow-tls))

从 Surge iOS 5.2.0 和 Surge Mac 4.10.0 开始，Surge 支持 Shadow TLS v2 协议。将 `shadow-tls-password` 附加到任何代理声明中即可使用它。

示例：

```ini
[Proxy]
STLS-SNELL = snell, 1.2.3.4, 443, psk=pwd1, version=4, reuse=true, shadow-tls-password=pwd2
```

从 Surge iOS 5.5.0 和 Surge Mac 5.0.3 开始，Surge 支持 Shadow TLS v3 协议。

示例：

```ini
STLS-SNELL = snell, 1.2.3.4, 443, psk=pwd1, version=4, reuse=true, shadow-tls-password=pwd2, shadow-tls-version=3
```

#### 参数

*   `shadow-tls-password`: 必填。它必须与服务器的设置匹配。
*   `shadow-tls-sni`: 可选。SNI 将在 TLS 握手期间以明文形式发送到服务器。如果未设置，将不发送任何 SNI。
*   `shadow-tls-version`: 可选。可能的值：2 或 3。默认值：2。
