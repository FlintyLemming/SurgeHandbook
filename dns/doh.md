# 加密 DNS

如果配置了加密 DNS，传统 DNS 将仅用于测试连通性以及解析加密 DNS URL 中的域名。

支持的协议：

*   DNS over HTTPS: `https://example.com`
*   DNS over HTTP/3: `h3://example.com`
*   DNS over QUIC: `quic://example.com`

### 为所有域名使用加密 DNS

```ini
[General]
encrypted-dns-server = https://8.8.8.8/dns-query
```

你可以在此处指定多个加密服务器，以逗号分隔。

### 为指定域名使用加密 DNS

```ini
[Host]
example.com = server:https://cloudflare-dns.com/dns-query
```

### 通过代理使用加密 DNS

如果你希望通过代理查询 DoH 服务器，可以将 `encrypted-dns-follow-outbound-mode` 设为 true。

```ini
[General]
encrypted-dns-follow-outbound-mode=true
```

所有的加密 DNS 连接将遵循出站模式设置。然后可以为 DoH 的主机名配置一条规则以使用代理。

或者，使用 `PROTOCOL,DOH`、`PROTOCOL,DOH3` 或 `PROTOCOL,DOQ` 规则来匹配所有加密 DNS 连接。
