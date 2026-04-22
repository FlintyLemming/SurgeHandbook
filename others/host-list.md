# Host List 参数类型

在 Surge 中，许多参数使用 Host List 类型来适应各种复杂的需求，例如 `force-http-engine-hosts`、`always-raw-tcp-hosts`、`[MITM]` 的 `hostname` 等。

Host List 类型的参数是一个由 `,` 分隔的列表，并遵循以下规则：

*   使用前缀 `-` 来排除一个主机名。
*   支持通配符 `*` 和 `?`。
*   列表中的项目将按顺序匹配，一旦匹配成功，就会结束匹配过程。因此，排在前面的项目具有更高的优先级。特别是当使用 `-` 前缀时，你应该把需要排除的主机名写在前面。
*   如果未提供端口号，Surge 会自动追加该参数的标准端口号，例如对于 `force-http-engine-hosts` 参数，如果仅配置了主机名，它将仅对 80 端口生效。对于 MITM 功能，它将仅对 443 端口生效。
*   使用后缀 `:port` 来匹配其他端口。
*   使用后缀 `:0` 来匹配所有端口。
*   使用 `<ip-address>` 直接匹配所有使用 IPv4/IPv6 地址而不是域名的主机名。
*   使用 `<ipv4-address>` 直接匹配所有使用 IPv4 地址而不是域名的主机名。
*   使用 `<ipv6-address>` 直接匹配所有使用 IPv6 地址而不是域名的主机名。

以 `force-http-engine-hosts` 参数为例：

*   `-*.apple.com`：排除所有发往 `*.apple.com` 的 80 端口请求。
*   `www.google.com`：对 `www.google.com` 的 80 端口使用强制 HTTP 处理。
*   `www.google.com:8080`：对 `www.google.com` 的 8080 端口使用强制 HTTP 处理。
*   `www.google.com:0`：对 `www.google.com` 的所有端口使用强制 HTTP 处理。
*   `*:0`：对所有主机名的所有端口使用强制 HTTP 处理。
*   `-<ip-address>`：排除所有直接使用 IPv4/IPv6 地址的请求。

### 示例

在为 MITM 配置 hostname 时，如果你想解密所有的 HTTPS 连接，但排除那些由于证书固定 (certificate pinning) 而无法解密的知名主机名，你可以这样写：

```ini
[MITM]
hostname = -*icloud*, -*.mzstatic.com, -*.facebook.com, -*.instagram.com, -*.twitter.com, -*dropbox*, -*apple*, -*.amazonaws.com, -<ip-address>, *
```
