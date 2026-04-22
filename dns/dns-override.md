# DNS 服务器

Surge 使用自定义的 DNS 客户端来支持高级功能。它的行为可能与操作系统的 DNS 客户端不同。

### 上游 DNS 服务器

Surge 默认使用操作系统的 DNS 服务器地址。你可以使用 `dns-server` 参数来覆盖它们。

```ini
[General]
dns-server = 8.8.8.8, 8.8.4.4
```

使用关键字 `system` 可以将操作系统的 DNS 服务器追加到列表中。（重复的服务器将被忽略）

```ini
[General]
dns-server = system, 8.8.8.8, 8.8.4.4
```

### 技术细节

Surge 会同时向所有的 DNS 服务器发起查询以提高性能，类似于 dnsmasq 配合 `--all-servers` 参数。最先返回的响应将被使用。Surge iOS 应用和 Surge Dashboard 会显示是哪个服务器最先响应的。如果 Surge 在 2 秒内没有收到任何响应，它会再次查询所有服务器。重试四次后，Surge 将放弃并报告 DNS 错误。

部分域名的权威名称服务器可能性能不佳，导致上游 DNS 服务器由于服务器端超时或其他连接问题返回空响应。如果**所有**上游 DNS 服务器显式返回空 DNS 响应，或者部分服务器返回空响应且其余服务器在 2 秒内没有响应，Surge 将报告空 DNS 错误。

当 IPv6 可用并开启时，Surge DNS 客户端将同时向上游 DNS 服务器发送 A 和 AAAA 请求。最先返回的 A 或 AAAA 响应将被使用。
