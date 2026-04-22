# HTTPS 解密 (中间人攻击, MitM)

Surge 可以通过 MitM 来解密 HTTPS 流量。有关更多信息，请参阅[维基百科的文章](https://en.wikipedia.org/wiki/Man-in-the-middle_attack)。

证书生成器可帮助你生成用于调试的新的 CA 证书，并使该证书被系统信任。它可在 Surge Dashboard（Mac 版）和 Surge iOS 配置编辑器中使用。此证书是在本地生成的，仅保存在你的配置文件和系统钥匙串中。新证书的密钥是使用 OpenSSL 随机生成的。

你也可以使用现有的 CA 证书。将证书导出为带有密码短语的 PKCS#12 格式 (`.p12`)。请注意，由于系统限制，密码短语不能为空。使用 `base64` 命令将其编码为 base64 字符串，并将以下设置追加到你的配置文件中。

```ini
[MITM]
ca-p12 = MIIJtQ.........
ca-passphrase = password
hostname = *google.com
h2 = true
```

Surge 仅对在此声明的主机解密流量。一个通用的配置可能如下所示：

`hostname = -*.apple.com, -*.icloud.com, *`

> 某些应用程序拥有使用固定证书 (pinned certificates) 或 CA 的严格安全策略。对这些主机启用解密可能会导致问题。

此参数属于 Host List 类型，有关详细规则请参见：[Host List 参数类型](../others/host-list.md)

## 选项

### skip-server-cert-verify (布尔值, 默认值: false)

在执行 MITM 时不验证远程主机的证书。

### h2 (布尔值, 默认值: false)

MITM over HTTP/2：通过 HTTP/2 协议执行 MITM 以解密 HTTPS 流量，这可以提高并发请求的性能。

### client-source-address

使用此参数可以仅在某些设备上启用 MITM 功能。

*   它是一个使用逗号作为分隔符的列表参数。
*   你可以指定单个 IP 地址，也可以使用 CIDR 块。同时支持 IPv4 和 IPv6。
*   你可以使用 `-` 前缀来排除某些客户端，例如：`client-source-address = -192.168.1.2, 0.0.0.0/0`
*   如果未设置此参数，则将对所有客户端启用 MITM。相当于 `client-source-address = 0.0.0.0/0, ::/0`
*   如果你希望对当前设备启用 MITM，则应包含 `127.0.0.1`。
*   从 Surge Mac 版本 6.1.0 开始，此参数可以使用 MAC 地址来匹配特定的客户端。

### auto-quic-block (布尔值, 默认值: true) iOS 5.8.0+ Mac 5.4.0+

当 QUIC 连接（即 HTTP/3）匹配 MITM 主机名列表时，将自动阻断该 QUIC 连接，迫使连接回退至 HTTP/2 或 HTTP/1.1，从而能够被 MITM 拦截。
