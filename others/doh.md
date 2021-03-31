# DNS over HTTPS

如果配置了 DNS-over-HTTPS，传统的 DNS 仅用于测试连接性和解析 DOH 地址里面的域名。

## 为所有域名使用 DoH

```
[General]
doh-server = https://9.9.9.9/dns-query
```

你可以指定多个 DNS-over-HTTPS 服务器（不推荐）。

## 为特定域名使用 DoH

```text
[Host]
example.com = server:https://cloudflare-dns.com/dns-query
```

## 如果直接使用 IP 地址

像下面这样设置即可：

```text
[General]
dns-server = https://9.9.9.9/dns-query
```

你可以指定多个 DoH 服务器（并不建议），甚至和普通 DNS 服务器地址放在一起。但由于 DoH 返回结果一般比普通 DNS 慢，所以反而会在并发查询中被丢弃。

## DNS over HTTPS 格式

对于 DoH 格式，有两种形式：JSON 和 DNS wireformat (RFC1035)。

你需要确定你的 DoH 服务支持哪一种形式。

* Surge iOS 4.1 和以下的版本 / Surge Mac 3.4.1 和以下的版本： 只支持 JSON 形式的文件。

* Surge iOS 4.2 和以上的版本 / Surge Mac 3.5.0 和以上的版本： Surge 默认使用 DNS wireformat，你也可以继续使用 JSON 文件。

    ```
    [General]
    doh-format=json
    ```
    
## 通过代理使用 DoH

如果你想通过代理使用 DoH，你可以打开 doh-follow-outbound-mode。

```
[General]
doh-follow-outbound-mode=true
```

所有的 DoH 会走出站模式的设置，然后再编辑规则，让 DoH 的域名走代理。

或者，使用 `PROTOCOL,DOH` 规则去匹配所有的 DoH 连接。