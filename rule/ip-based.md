# 基于 IP 的规则 (IP-based Rule)

共有 3 种基于 IP 的规则类型。如果请求的主机名是域名，基于 IP 的规则将触发 DNS 查询。如果 DNS 查询失败，Surge 将中止规则测试并报告错误。

#### IP-CIDR

```ini
IP-CIDR,192.168.0.0/16,DIRECT
IP-CIDR,10.0.0.0/8,DIRECT
IP-CIDR,172.16.0.0/12,DIRECT
IP-CIDR,127.0.0.1/8,DIRECT
```

如果请求的 IP 地址匹配指定的范围，则触发该规则。

从 Surge Mac 6.0.0 开始，你也可以提供一个不带 `/` 掩码的单一 IPv4 地址，它将被视为 `/32`。

#### IP-CIDR6

```ini
IP-CIDR6,2001:db8:abcd:8000::/50,DIRECT
```

如果请求的 IPv6 地址匹配指定的范围，则触发该规则。

也支持单一 IPv6 地址——例如，写入 `IP-CIDR6,2404:6800::` 等同于 `/128`。

#### GEOIP

`GEOIP,US,DIRECT`

如果 GeoIP 测试结果匹配指定的国家/地区代码，则触发该规则。

#### IP-ASN

`IP-ASN,1234,DIRECT`

如果远程 IP 地址的自治系统编号 (ASN) 匹配，则触发该规则。

### 基于 IP 的规则参数

#### no-resolve

```ini
GEOIP,US,DIRECT,no-resolve
IP-CIDR,172.16.0.0/12,DIRECT,no-resolve
```

当遇到 `GEOIP` 或 `IP-CIDR` 规则时，Surge 将发送 DNS 查询以检查请求的主机名是否为域名。你可以选择 `no-resolve` 选项，让带有域名的请求跳过该规则。

> 注意：如果某些域名无法被本地 DNS 服务器解析，请确保在匹配该域名的规则之前没有基于 IP 的规则。否则，规则测试将由于 DNS 错误而失败。你也可以使用 `no-resolve` 来解决此问题。
